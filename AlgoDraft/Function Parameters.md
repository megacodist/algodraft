---
status: Draft
---
Functions rarely operate in a vacuum; they usually need some input data to process, and they often need to communicate results back. **Parameters** are the named placeholders defined within a function's signature that allow it to receive data from (and sometimes send data back to) the part of the algorithm that calls it (the "caller").

When you define a function, you specify its parameters within the parentheses () that follow the function name. Each parameter definition outlines its contract with the caller.

**Syntax**: The general syntax for a single parameter definition is:

```
[IN | OUT | INOUT] $paramName AS DataType [<- <defaultValue>]
```

- `[IN | OUT | INOUT]`: The optional [[#Parameter Directionality (`IN`, `OUT`, `INOUT`)|Parameter Directionality]], defaults to `IN`.
    
- `$paramName`: The identifier (name) used to refer to this parameter inside the body of the routine. It must be conformed to variable naming rules.
    
- **AS DataType**: This is a mandatory clause that explicitly declares the expected data type of the parameter (e.g., `Integer`, `String OR DateTime`, `List<Float>`, `UserRecord`). This helps ensure that the function receives the kind of data it's designed to work with.

- **`[<- defaultValue]`**: This is an optional part that allows you to specify a default value for the parameter, making it optional for the caller.

**Example:** Parameter List:

```
FUNCTION ProcessUserData (
    IN $userId AS Integer,                 // Required input: the user's ID
    INOUT $userData AS UserRecord,         // Input/Output: user data to be read and possibly modified
    OUT $statusMessage AS String,          // Output: a message indicating the outcome
    IN $logLevel AS Integer <- 1           // Optional input: logging verbosity, defaults to 1
	) -> Boolean :=                        // Function returns a success/failure flag

    // ... function body ...
ENDFUNCTION
```

In this example, `$userId`, `$userData`, `$statusMessage`, and `$logLevel` are all parameters.

# Default-Valued Parameters

Sometimes, a parameter might have a common or typical value that you want to use if the caller doesn't explicitly provide a different one. AlgoDraft allows you to define **default values** for parameters, making them optional during function invocation.


- **Syntax:** To specify a default value, you append `<- <defaultValue>` to the parameter definition.
    
    ```
    [IN | OUT | INOUT] $paramName AS DataType <- <defaultValue>
    ```

- **Behavior:**
    
    - If the caller provides an argument for this parameter, the caller's value is used.
    
    - If the caller omits the argument for this parameter, the `<defaultValue>` specified in the function definition is automatically used for that parameter within the function.

- **Placement Rule:** For clarity, especially when using positional arguments, it is required to place parameters with default values after parameters without default values in the parameter list.

**Example 1:** Simple Default for an `IN` Parameter:

```
FUNCTION GetGreeting(
    IN $name AS String,
    IN $greeting AS String <- "Hello" // $greeting is optional, defaults to "Hello"
	) -> String :=
	
    RETURN $greeting + ", " + $name + "!"
ENDFUNCTION


// Calling Greet:
Greet("Alice")                // Returns: Hello, Alice! ($greeting uses default)
Greet("Bob", "Hi")            // Returns: Hi, Bob! ($greeting is "Hi")
Greet($greeting <- "Welcome", $name <- "Charlie") // Output: Welcome, Charlie!
```

**Example 2: Default for an `OUT` Parameter (Less Common, but Possible):**

While `OUT` parameters are primarily for sending data out, a default could represent an initial or fallback state if, for some reason, the function's logic doesn't assign a new value (though `OUT` parameters are usually expected to be assigned).

```
FUNCTION CheckSystem (
	    OUT $statusCode AS Integer <- -1 // Default status if no specific check applies
	) :=

    // ... complex logic that might or might not set $statusCode ...
    IF {{ some condition }} THEN
        $statusCode <- 0 // Success
    // If no condition sets $statusCode, it retains its default (or initial value if assigned by caller)
    // Good practice: always ensure OUT parameters are explicitly set if they represent a result.
    ENDIF
ENDFUNCTION
```

**Note**: It's generally better practice to ensure `OUT` parameters are always explicitly assigned a meaningful value by the function before it exits, rather than relying on defaults for output values.

**Example 3: Multiple Default-Valued Parameters:**

```
FUNCTION Connect(
      $host AS String           // Required parameter
      $port AS Integer <- 8080  // Optional parameter, defaults to 8080
      $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ) :=
   
   // ... function body ...
ENDFUNCTION
```

If the caller does not provide an argument corresponding to `$port` or `$timeout` when calling Connect, the function will automatically use `8080` for `$port` and `5000` for `$timeout` internally.

Default-valued parameters significantly increase the flexibility of your functions, allowing them to handle common cases with less explicit input from the caller.
# Parameter Directionality (`IN`, `OUT`, `INOUT`)

A crucial aspect of defining function parameters in AlgoDraft is specifying their **directionality**. This explicitly declares the intended direction of data flow for that parameter between the caller and the function, forming a clear contract. This is optional and defaults to `IN`.

This approach is vital for designing algorithms because it makes the intent and potential side effects of a function call immediately clear from its signature.
## `IN` Directionality

- **Purpose:** Specifies that the parameter is used solely to pass input data **into** the function. It is the default directionality of parameters in AlgoDraft.

- **Data Flow:** Information flows one way: from the caller's argument to the function's parameter when the function is called.

- **Caller Expectation:** The caller's original literal, variable, expression, or anything else that is evaluated to a value, provided as an argument, will **not** be changed by the function through this `IN` parameter. The function operates on a conceptual copy or an immutable view of the input.

- **Function Behavior:** Inside the function, an `IN` parameter can be read but should not be assigned a new value that is intended to be seen by the caller (any such assignment would be local to the function).

- **Use Case:** Passing values needed for computation, configuration settings, data to be examined without alteration. This is the most common directionality for inputs.

**Example:**

```
FUNCTION CalculateArea (
	    $length AS Float, // Input: length of a rectangle
	    IN $width AS Float   // Input: width of a rectangle
	) -> Float :=
    RETURN $length * $width
ENDFUNCTION
```

## `OUT` Directionality:

- **Purpose:** Specifies that the parameter is used solely to pass output data **out of** the function, back to a variable provided by the caller.

- **Data Flow:** Information flows one way: from the function's parameter to the caller's argument variable when the function completes or returns.

- **Caller Expectation:** The caller must provide a variable as the argument for an `OUT` parameter. The initial value of this caller variable is generally disregarded by the function. The caller should expect this variable's value to be **overwritten** with the result assigned to the `OUT` parameter by the function.

- **Function Behavior:** Inside the function, an `OUT` parameter should be treated as uninitialized initially (conceptually). The function is responsible for **assigning a value** to this parameter before it finishes. Reading from an `OUT` parameter before assigning it a value is generally an error or leads to undefined behavior.

- **Use Case:** Returning secondary results, status indicators, error messages, or multiple distinct pieces of output when a primary `RETURNS` value is not used or is insufficient.

```
FUNCTION GetUserDetails (
	    $userId AS Integer,          // Input: ID of the user
	    OUT $userName AS String,        // Output: The user's name
	    OUT $userAge AS Integer         // Output: The user's age
	) :=
    // Simulate fetching data
    IF $userId = 1 THEN
        $userName <- "Alice"
        $userAge <- 30
    ELSE
        $userName <- "Unknown"
        $userAge <- -1
    ENDIF
ENDFUNCTION

// Caller:
DECLARE $name AS String
DECLARE $age AS Integer
GetUserDetails(1, $userName <- $name, $userAge <- $age)
// Now $name is "Alice" and $age is 30
```

## `INOUT` Directionality

- **Purpose:** Specifies that the parameter serves as both input **into** the function and allows the function to pass modified data **out of** the function, back to the same variable provided by the caller.

- **Data Flow:** Information flows two ways. The parameter initially receives the value from the caller's argument. The function can then read this value and modify it. Upon completion, the (potentially modified) final value of the parameter is reflected back in the caller's original variable.

- **Caller Expectation:** The caller provides a variable with an initial value. The caller should expect this variable to potentially be **modified** by the function.

- **Function Behavior:** Inside the function, an `INOUT` parameter has an initial value from the caller. The function can read from it and assign new values to it.

- **Use Case:** Modifying data structures passed by the caller (e.g., sorting a list provided by the caller "in-place"), updating counters, accumulating results directly in a caller's variable.


```
FUNCTION IncrementValue (
	    INOUT $counter AS Integer, // Input/Output: the value to increment
	    IN $amount AS Integer <- 1 // Input: how much to increment by
	) :=
    $counter <- $counter + $amount
ENDFUNCTION

// Caller:
$myScore AS Integer <- 100
IncrementValue($myScore) // $amount uses default 1
// Now $myScore is 101
IncrementValue($myScore, 5)
// Now $myScore is 106
```

## Summary

By consistently using `IN`, `OUT`, and `INOUT`, your function signatures in AlgoDraft become highly self-documenting, making it easier to reason about the behavior and data flow of your algorithms.

The following table highlights deafferents among parameter directionalities:

| Mode    | Purpose          | Data Flow Direction | Initial Value Usage | Caller Variable Effect |
| ------- | ---------------- | ------------------- | ------------------- | ---------------------- |
| `IN`    | Input Only       | Caller -> Routine   | Read by Routine     | Not Affected           |
| `OUT`   | Output Only      | Routine -> Caller   | Ignored by Routine  | Updated on Exit        |
| `INOUT` | Input and Output | Caller <-> Routine  | Read by Routine     | Updated on Exit        |
