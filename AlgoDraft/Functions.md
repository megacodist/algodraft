---
status: Draft
---
Functions compute and **return** a single value based on inputs or perform actions, cause side effects, possibly modifying state.

# Declaration Syntax
Functions are defined using the `FUNCTION` keyword, followed by a name, a parameter block (`PARAMS`/`ENDPARAMS`), an optional return type declaration, the body of the routine, and finally `ENDFUNCTION`.
```
FUNCTION FunctionName [RETURNS ReturnType]
   PARAMS
      <param1>
      <param2>
      ...
      <paramN>
   ENDPARAMS
   // Body
ENDFUNCTION
```

* `FUNCTION`: Begins the definition of any routine (whether it acts as a mathematical-like function or a procedure).
* `FunctionName`: The identifier (name) you give to the routine. This name is used to call the routine later. It should follow [[Appendix 2, Naming conventions#PascalCase or UpperCamelCase|PascalCase]] convention.
* `[RETURNS ReturnType]` (Optional clause): The key difference between a value-returning function and an action-performing procedure lies in the optional `RETURNS` clause:
   * If present, this clause specifies that the routine is a function designed to return a value.
   * If absent, the routine is a procedure designed primarily for its actions/side effects.
* `PARAMS`: The keyword that begins the parameters block where all the routine's parameters (arguments it accepts) are defined.
* `ENDPARAMS`: The keyword that marks the end of the parameter definition block.
* `// Body`: A placeholder representing the sequence of statements and logic that make up the routine's implementation. This is where computations happen, actions are performed.
* `ENDFUNCTION`: The keyword that marks the end of the entire function definition block.

## Parameters syntax
One or more parameters can be declared using the following syntax. If function does not take any parameter, the whole `PARAMS`/`ENDPARAMS` block must be omitted.
```
[IN | OUT | INOUT] $paramName AS DataType [<- <defaultValue>]
```
* `[IN | OUT | INOUT]`: Optional [Parameter Directionality](#parameter-directionality-in-out-inout)
* `$paramName`: The identifier (name) used to refer to this parameter inside the body of the routine.
* `AS DataType` (mandatory): Declares the expected data type for the parameter.
* `[<- <defaultValue>]` (Optional default value):
   * If **present**, this argument becomes optional for the caller. If the caller doesn't provide a value/argument for this parameter, it will automatically be initialized with the `defaultValue`.
   * Arguments with default values must come after arguments without default values in the list.

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
