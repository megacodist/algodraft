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
* Data is conceptually transferred from the caller's argument to the routine's parameter at the start of the call.
 * Any change made to the parameter, remains inside of the routine and is NOT reflected back to the caller.
 * The caller's argument can be a literal, a variable, an expression, or anything else that is evaluated to a value.
 * Use cases include passing values needed for computation, configuration settings, or data to be read without modification.

**Example**:
   ```
PARAMS
    IN $config1 AS String // Input configuration #1
	$config2 AS String    // Input configuration #2
ENDPARAMS
   ```

## `OUT` Directionality:
* Data is conceptually transferred from routine's parameter to the the caller's argument at the end of the call.
* The caller's argument must only be a variable and the initial value of the caller's variable is generally disregarded; the routine is responsible for assigning the output value.
* Use cases include returning secondary results, status indicators, or outputs when a primary `RETURN $value` is not used or insufficient.

 ```
 PARAMS
     OUT $statusMessage AS String // Output status description
     OUT $resultCode AS Integer  // Output numerical result code
 ENDPARAMS
 ```

## `INOUT` Directionality
*  Data is initially transferred from the caller's argument to the routine's parameter. The routine can then read this value and modify it. Upon completion, the final value of the parameter is transferred back to the caller's argument.
* The caller's argument must only be a variable.
* Use cases include modifying data structures passed by the caller (e.g., sorting a list in place), updating counters or state variables provided by the caller.

```
PARAMS
     INOUT $dataList AS List<Person> // Input data, potentially modified by routine
ENDPARAMS
```

## Summary
| Mode | Purpose | Data Flow Direction | Initial Value Usage | Caller Variable Effect |
|------|---------|---------------------|---------------------|-----------------| 
| IN |Input Only | Caller -> Routine | Read by Routine | Not Affected |
|OUT | Output Only | Routine -> Caller | Ignored by Routine | Updated on Exit |
| INOUT | Input and Output | Caller <-> Routine | Read by Routine | Updated on Exit |
