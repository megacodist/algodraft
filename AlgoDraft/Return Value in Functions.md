---
status: Draft
---
In AlgoDraft, the `FUNCTION` keyword is used to define all reusable blocks of code, often called routines, subroutines, procedures, or functions. A key characteristic of any `FUNCTION` is how it communicates results back to the part of the code that called it (the caller). Our syntax provides flexibility to handle three primary scenarios:

1. **No Return Value (Procedures):** Sometimes, a routine's main purpose is to perform an action, cause a side effect (like printing to the screen, modifying a file), or update state through parameters, rather than computing a new value to be used directly by the caller. These are often called **procedures**. In AlgoDraft syntax, you define such routines by simply **omitting** the `RETURNS` clause in the `FUNCTION` definition. Their "result" is the action they perform.

2. **Returning One Value (Traditional Functions):** This is the classic function pattern where the routine's primary purpose is to perform a computation and send a single result back to the caller. This result can then be used in further calculations or assignments. To define such a routine, you **must include** the `RETURNS DataType` clause, specifying the type of the value being returned. Inside the function body, the `RETURN` value statement is used to send the computed result back.

3. **Returning Multiple Values:** Often, a single computation logically produces several related results (e.g., finding both a minimum and maximum value, calculating a result and an associated error code). The `FUNCTION` syntax supports mechanisms to handle this need, allowing multiple pieces of information to be effectively communicated back to the caller. We will explore the specific techniques for achieving this – namely, returning structured data types and using parameters with specific directionality (OUT, INOUT) – in later sections.

The following sections will detail how to define the parameters a FUNCTION accepts, how to invoke (call) these routines, and delve deeper into the mechanisms for returning single or multiple values.
# Procedures: no return value
A procedure is a routine that primarily performs actions or causes side effects (like printing to the screen, modifying a file, updating a global variable), or modifying an `OUT`/`INOUT` argument. It doesn't return a computed value back to the caller. To define a procedure, simply omit the `[RETURNS DataType]` part.

It is forbidden to return a value in a procedure using `RETURN $value` but it is possible to employ `RETURN` to exit the procedure any time.

**Example**
```
// Example: Procedure to display a formatted message
FUNCTION LogMessage
   PARAMS
      IN $message AS String         // Input: The text to show
      IN $prefix AS String <- "[INFO]" // Optional input: A prefix, defaults to "[INFO]"
   ENDPARAMS

   // Body: Performs the action
   $msg AS String <- $prefix + " " + $message
   DO {{log $msg}}
   // No RETURN statement needed (or use bare RETURN for early exit)

ENDFUNCTION // No RETURNS clause
```

# Functions with only one return value
A function is a routine whose primary purpose is to compute a value and return it to the caller. The name "function" and invocation syntax is inspired by mathematics.

To define a function, you must include the `RETURNS ReturnType` part, specifying the type of the value that will be returned. Inside the function's body, the flow of execution must always reach to a `RETURN` statement followed by the value (or an expression) compatible with `ReturnType` in declaration to send the result back.

**Example**
```
FUNCTION AddNumbers RETURNS Integer // Specifies that an Integer value will be returned
   PARAMS
      $num1 AS Integer
      IN $num2 AS Integer
   ENDPARAMS

   // Body: Computes the value
   $sum AS Integer
   $sum <- $num1 + $num2
   RETURN $sum // Returning the computed value

ENDFUNCTION
```

# Functions with multiple return values
Sometimes, a computation needs to produce more than one result (e.g., finding both the minimum and maximum value, performing an operation and returning a result plus a status code). AlgoDraft supports two primary methods for achieving this:

## Method 1: Returning a Single Structured Data Type

This is the most common approach when a function's primary purpose is to compute a set of related values. Define the function with a `RETURNS` clause specifying a structured data type (like a `Tuple`, `Record`,  or `List`) that can hold multiple individual values. Inside the function, package the results into an instance of this structure and use a single `RETURN` statement.

- **Pros:** Keeps the function signature focused on a single return value (albeit a complex one). Logically groups related return values. Often aligns well with functional programming paradigms.

- **Cons:** Requires defining or using suitable structured types. Caller needs to unpack or access components from the returned structure.

**Example 1**:
```
// Example: Function returning both min and max of a list
FUNCTION FindMinMax RETURNS Tuple<Integer, Integer>  // Specifies returning a pair
    PARAMS
        IN $numbers AS List<Integer> // Input: List to search
    ENDPARAMS
    
    // Assume list is not empty for simplicity
    $min AS Integer <- $numbers[0]
    $max AS Integer <- $numbers[0]
    FOR EACH $num IN $numbers DO
        IF $num < $min THEN $min <- $num ENDIF
        IF $num > $max THEN $max <- $num ENDIF
    ENDFOR
    
    RETURN ($min, $max,) // Returning a 2-tuple (pair)...
ENDFUNCTION


// Calling FindMinMax
$data AS List<Integer> <- [4, 1, 8, 3]
$minMaxResult AS Tuple<Integer, Integer>
$minMaxResult <- FindMinMax($data)

// Accessing tuple elements...
NOTIFY {{the minimum is $minMaxResult[0]}}
NOTIFY {{the maximum is $minMaxResult[1]}}

// Or, using tuple unpacking...
$minValue, $maxValue AS Integer
$minValue, $maxValue <- UNPACK $minMaxResult
NOTIFY {{the minimum is $minValue}}
NOTIFY {{the maximum is $maxValue}}
```

**Example 2:**
```
// Example: Returning user data using a Record (assuming Record type exists)
RECORD UserData
    $name AS String
    $age AS Integer
    $isActive AS Boolean
ENDRECORD


FUNCTION GetUserDetails RETURNS UserData
    PARAMS
        IN $userId AS Integer
    ENDPARAMS
    
    // ... logic to fetch user data ...
    DECLARE $userName AS String <- "Alice"
    DECLARE $userAge AS Integer <- 30
    DECLARE $userStatus AS Boolean <- TRUE
    
    // Create and return the record
    RETURN {name: $userName, age: $userAge, isActive: $userStatus}
ENDFUNCTION


// Calling GetUserDetails
$userData AS UserData <- GetUserDetails(123)
NOTIFY {{user data is $userData}}
```

## Method 2: Using OUT and INOUT Parameter Directionalities

This approach uses parameters not just for input, but explicitly to send results back to variables provided by the caller. Define the function with one or more parameters marked with `OUT` or `INOUT` directionalities. Inside the function, assign the computed results to these parameters. The function may also have a `RETURNS` clause for a primary return value, or it may have no `RETURNS` clause if all results are passed back via parameters (acting more like a procedure).

**Example 1:**
```
// Returning min and max via OUT parameters...
// No RETURNS clause - results are solely via OUT parameters
FUNCTION FindMinMax_V2
    PARAMS
        IN $numbers AS List<Integer>
        OUT $minResult AS Integer     // Output parameter for minimum
        OUT $maxResult AS Integer     // Output parameter for maximum
    ENDPARAMS
    
    // Assume list is not empty
    $minResult <- $numbers[0] // Assign directly to OUT parameter
    $maxResult <- $numbers[0] // Assign directly to OUT parameter
    FOR EACH $num IN $numbers DO
        IF $num < $minResult THEN $minResult <- $num ENDIF
        IF $num > $maxResult THEN $maxResult <- $num ENDIF
    ENDFOR
    // No RETURN statement needed for OUT parameters
ENDFUNCTION


// Calling FindMinMax_V2
$data AS List<Integer> <- [4, 1, 8, 3]
$minValue AS Integer     // Variable to receive minimum
$maxValue AS Integer     // Variable to receive maximum
FindMinMax_V2($data, $minResult <- $minValue, $maxResult <- $maxValue)
NOTIFY {{minimum and maximum are $minValue and $maxValue respectively}}
```

**Example 2:**
```
// Performing an action and returning status/result via OUT & RETURN

// Primary result: TRUE if successful, FALSE otherwise
FUNCTION ProcessItem RETURNS Boolean 
    PARAMS
        IN $itemId AS Integer
        OUT $status AS String // Secondary result via OUT
    ENDPARAMS
    
    // ... logic to process item ...
    TRY
        DO {{}}
    CATCH
        status <- "An error occurred"
        RETURN FALSE
    ELSE
        status <- "successful"
        RETURN TRUE
    ENDTRY
ENDFUNCTION


// Calling ProcessItem
$status AS String // Variable to receive processed value
$successFlag AS Boolean
$successFlag <- ProcessItem($itemId <- 456, $status <- $status)
IF $successFlag THEN
    NOTIFY {{the process was successful as success}}
ELSE
    NOTIFY {{an error occurred with $statumessages as warning}}
ENDIF
```
