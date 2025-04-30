---
status: Draft
---
# Stream Functions: Lazy Generation

In AlgoDraft, a **Stream Function** is a special type of function defined using the `STREAM` keyword. Unlike regular functions that compute a result and return it all at once, a Stream Function describes a process that can generate a sequence of values on demand, one value at a time, without needing to produce or store the entire sequence upfront. This concept is known as **lazy generation**.

Think of a Stream Function not as a container holding data, but as a recipe or a set of instructions for how to create the next piece of data in a sequence whenever it's requested.

## The Core Idea: On-Demand Production and State

The power of Stream Functions comes from two key ideas:

1. **On-Demand Production:** Values are generated only when explicitly asked for.

2. **State Preservation:** Between yielding values, the Stream Function pauses its execution but remembers exactly where it left off and the state of its local variables. The next time a value is requested, it resumes from that paused point.

This makes Stream Functions ideal for generating potentially very large or even infinite sequences efficiently, as only the currently needed value (and the stream's small state) resides in conceptual memory at any given moment.

# Defining a Stream Function

You define a Stream Function using the `STREAM` keyword:

```
STREAM StreamName YIELDS YieldType // Declares the type of items yielded
    PARAMS
        $param1 AS DataType1       // Parameter definitions
        IN $param2 AS DataType2 <- defaultValue
        // ... Only IN parameters are allowed for Streams ...
    ENDPARAMS
	
	// Body: Contains logic, local variables, and YIELD statements
	// ... Code executes incrementally ...
	
	YIELD valueOfTypeYieldType // Produces the next value and pauses
	
	// ... More code ...
	
	EXHAUST // Optional: Manually terminate the stream
	
	// ... More code ...
	
	// Reaching ENDSTREAM terminates the stream
ENDSTREAM
```

- **`STREAM StreamName`**: Gives the function a name.

- **`YIELDS <YieldType>`**: Specifies the conceptual type of the values this stream function is designed to produce. While not always strictly required in pseudocode if the type is obvious, **explicitly stating the type (like `YIELDS Integer`, `YIELDS String`, `YIELDS Customer`) is highly recommended** for clarity and to define the Stream Function's output contract.

- **`PARAMS ... ENDPARAMS`**: The parameters to declare input parameters, similar to regular functions. These parameters are available within the stream function's body.
	
	- Only `IN` [Parameter Directionality](#parameter-directionality-in-out-inout) is permitted for parameters defined within a stream's `PARAMS` block. The multi-stage, suspend/resume nature of streams makes the semantics of `OUT` and `INOUT` parameters (which typically relate to the single completion state of a routine) complex and counter-intuitive in this context. Inputs configure the stream's generation process; outputs are exclusively handled via `YIELD`.
	
	- If the stream function does not take any parameter, this block must be removed completely.

- **`<Body>`**: Contains the pseudocode logic that will be executed. This body will contain `YIELD` and potentially `EXHAST` statements for pre-mature termination of the stream.

- **`ENDSTREAM`**: Marks the end of the Stream Function definition.

# Yielding Values: The YIELD Keyword

The core operation within a Stream Function's body is the `YIELD` keyword.

- **`YIELD value`**: When the execution flow in the stream function reaches a `YIELD` statement:
    
    - The value specified is produced and conceptually sent out.
    
    - The Stream Function's execution is **paused** immediately after the `YIELD` statement.
    
    - The Stream Function's entire internal state, **including the values of all local variables** defined within its body, is conceptually saved.

The next time the stream is driven forward (as explained later), execution will resume precisely from the statement after the `YIELD` that was just executed, with all variables retaining their saved values.

**Example:**
```
STREAM CountUpTo YIELDS Integer
    PARAMS
        $limit AS Integer
    ENDPARAMS
	
    $currentNum AS Integer <- 1

    WHILE $currentNum <= $limit DO
        YIELD $currentNum    // Yield the current number
        $currentNum <- $currentNum + 1 // Prepare for the next number
    ENDWHILE

    // When the WHILE loop finishes, the stream's logic reaches ENDSTREAM
    // This implicitly signals termination.

ENDSTREAM
```

# Terminating a Stream Function

A Stream Function finishes producing values (signaling to its consumer that there are no more items) in one of two conceptual ways from within its body:

1. **Implicit Termination (Reaching `ENDSTREAM`):**
    
    - If the stream function's logic executes and naturally reaches the `ENDSTREAM` marker (e.g., a loop condition becomes false, the code finishes a sequence of statements) without encountering any subsequent `YIELD` that would be reached on a future request, the stream conceptually signals exhaustion.
    
2. **Explicit Termination (`EXHAST`):**
    
    - The `EXHAST` keyword is used to immediately stop the stream function's execution at any point, regardless of whether the logic would otherwise continue or reach `ENDSTREAM`.
    
    - This is used for early termination based on a specific condition or logic branch.

**Example:**

```
STREAM SquaresUntilLimit YIELDS Integer
    PARAMS
        $maxValue AS Integer
    ENDPARAMS
	
    $currentBase AS Integer <- 1
	
    WHILE TRUE DO // Loop potentially indefinitely, stopped by EXHAST
        $square AS Integer <- $currentBase * $currentBase
		
        IF $square > $maxValue THEN
            NOTIFY {{the current square exceeds limit. stream stopped.}}
            EXHAST // Explicitly terminate the stream function
        ENDIF
		
        YIELD $square // Yield the square if it's within the limit
		
        $currentBase <- $currentBase + 1
    ENDWHILE
    // ENDSTREAM is conceptually reached if EXHAST is not hit,
    // also signaling termination (though unreachable in this specific WHILE TRUE example)
ENDSTREAM
```

# Consuming a Stream Function: Getting an Iterator

Calling a Stream Function definition **does not** immediately execute the code inside its body. Instead, calling a Stream Function **creates and returns a new, independent Iterator object** that is linked to that stream function's logic.

```
// Call the Stream Function, providing parameters if it has any
// This creates an Iterator instance
$myCounterIter AS Iterator<Integer> <- CountUpTo(5)

// Create another independent iterator from the same definition
$anotherCounterIter AS Iterator<Integer> <- CountUpTo(3)

// $myCounterIter and $anotherCounterIter are separate,
// they maintain their own state independently.
```

Each call to a Stream Function creates a brand new Iterator instance with its own fresh state, independent of any other iterators created from the same definition.

The actual execution of the Stream Function's body (hitting YIELD or termination) is **driven solely by operations performed on the associated Iterator**. Specifically, it's the NEXT operator on the Iterator that requests the next value and causes the Stream Function's logic to run from its last paused point until the next YIELD or termination signal (EXHAST or ENDSTREAM).