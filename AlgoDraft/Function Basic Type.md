---
status: Writing
---
In AlgoDraft, functions are treated as **first-class citizens**. This is a powerful concept meaning that a function is not just a static block of code but can be handled like any other piece of data. A first-class function can be:

*   Assigned to a variable.
*   Passed as an argument to another function.
*   Returned as a result from another function.

This capability is essential for designing sophisticated and flexible algorithms, especially for patterns like event handling, callbacks, or strategy patterns. The **`Function` basic type** is the mechanism in AlgoDraft that makes this possible. It is used to **annotate** variables, parameters, and return types that are expected to hold a "callable" entity, such as a function name or an instance method.

# Function Type Annotation

To ensure type safety and clarity, you can annotate a variable or parameter with a specific function signature. This defines the expected parameters and return type of any function that can be assigned to it. This is often referred to as a "function type" or "delegate type."

**Syntax**:

AlgoDraft uses a generic-style syntax for function type annotations:
```
Function<(paramType1, paramType2, ..., paramTypeN) -> ReturnType>
```

**Breakdown:**
*   `Function`: The type name identifying this as a function type.
*   `<...>`: Angle brackets enclose the entire signature, treating it as a generic type parameter for the `Function` type.
*   `(paramType1, ...)`: A comma-separated list of the **parameter types** that the function must accept.
*   `->`: The arrow operator, which separates the parameter list from the return type.
*   `ReturnType`: The data type of the value the function is expected to return.

# Syntactic Variations and Shorthands

AlgoDraft provides several shorthands to make function type annotations more concise and readable in common scenarios.

## Single Parameter (Similarity to `Mapping`)

If the function takes only one parameter, the inner parentheses for the parameter list can be omitted. This creates an elegant syntactic parallel to the `Mapping` type annotation.
**Syntax:**

```
Function<paramType1 -> ReturnType>
```

**Compare:**

```algodraft
myMap AS Mapping<Integer -> String>        // A map from Integer keys to String values
myCallback AS Function<Integer -> String>  // A function from an Integer to a String
```

## No Parameters

If the function takes no parameters, an empty parameter list `()` is used.
**Syntax:**

```
Function<() -> ReturnType>
```

## No Return Value (Procedure/Void Function)

If the function does not return a value, the special keyword **`NULL`** is used as the `ReturnType`. In AlgoDraft, `NULL` usually signifies the absence of a value.

**Syntax:**

```
Function<(paramType1, paramType2) -> NULL>
```

## No Parameters and No Return Value

**Syntax:**

```
Function<() -> NULL>
```

## Unspecified Signature

For early-stage design or highly dynamic scenarios where the exact signature is not yet known or important, a variable can be annotated simply as `Function`. This signifies that it can hold *any* callable entity but provides no information about its specific parameters or return type.

**Syntax:**

```
cb AS Function
```

**Use Case:**

This is analogous to an `ANY` type but is restricted to callable entities. It sacrifices compile-time/design-time signature checking for maximum flexibility.

```algodraft
dynamicCallback AS Function
// 'dynamicCallback' can hold any function, but calling it requires the
// designer to ensure the arguments are correct based on context.
```

**Examples of `Function` Type Annotations**

```algodraft
// A variable for a function that takes two Integers and returns a Boolean
comparisonFunc AS Function<(Integer, Integer) -> Boolean>

// A variable for a procedure (no return value) that takes one String
logger AS Function<String -> NULL> // Uses single-parameter shorthand

// A variable for a function that takes no parameters and returns a String
greetingProvider AS Function<() -> String>

// A variable for an action that takes no parameters and returns nothing
simpleAction AS Function<() -> NULL>

// A variable for any generic callable entity
anyCallable AS Function
```

# Use Cases

**a. Variable Declarations and Assignment**

You can declare a variable of a function type and assign any function with a compatible signature to it. Once assigned, the variable can be called just like the original function.

```algodraft
// A defined function
FUNCTION Add(IN a AS Integer, IN b AS Integer) -> Integer := RETURN a + b ENDFUNCTION

// A variable of a compatible function type
mathOperation AS Function<(Integer, Integer) -> Integer>

// Assigning the function to the variable
mathOperation <- Add

// Calling the function through the variable
result AS Integer <- mathOperation(10, 5) // result is 15
NOTIFY {{The result of the math operation is @result}}
```

**b. Functions as Parameters (Higher-Order Functions)**

A primary use of `Function` types is to create **higher-order functions**—functions that accept other functions as parameters. This is a powerful pattern for creating flexible and reusable algorithms, such as callbacks, predicates for filtering, or custom sorting logic.

**Example: A list filtering function**
```algodraft
// This function filters a list based on a "predicate" function it receives.
FUNCTION FilterList(
        IN sourceList AS List<Integer>,
        IN predicate AS Function<Integer -> Boolean> // Accepts a function as a parameter
	    ) -> List<Integer> :=
    resultList AS List<Integer> <- [] // Assumes NEW List() for empty
    FOR EACH item IN sourceList DO
        // Call the provided predicate function
        IF predicate(item) = TRUE THEN
            resultList.Add(item) // Assume Add is the method for List mutation
        ENDIF
    ENDFOR
    RETURN resultList
ENDFUNCTION

// A specific predicate function that matches the required signature
FUNCTION IsEven(IN num AS Integer) -> Boolean := RETURN (num MOD 2) = 0 ENDFUNCTION

// Using the higher-order function
numbers AS List<Integer> <- [1, 2, 3, 4, 5, 6]
evenNumbers AS List<Integer> <- FilterList(numbers, IsEven) // Pass IsEven as an argument
NOTIFY {{The filtered even numbers are @evenNumbers}} // User informed list is [2, 4, 6]
```

**c. Function Return Values (Function Factories)**

A function can also create and return another function. This is a useful pattern for creating "function factories" that generate specialized functions based on some initial input.

**Example: A function that creates multiplier functions**
```algodraft
// This implies AlgoDraft supports closures: the inner function 'Multiplier'
// "remembers" the 'factor' variable from its parent's scope.
FUNCTION CreateMultiplier(IN factor AS Integer) -> Function<Integer -> Integer> :=
    FUNCTION Multiplier(IN n AS Integer) -> Integer :=
        RETURN n * factor
    ENDFUNCTION
    RETURN Multiplier // Return the inner function itself
ENDFUNCTION

// Using the function factory to create specialized functions...
timesThree AS Function<Integer -> Integer> <- CreateMultiplier(3)
timesTen AS Function<Integer -> Integer> <- CreateMultiplier(10)

result1 AS Integer <- timesThree(5) // result1 is 15
result2 AS Integer <- timesTen(5)  // result2 is 50
NOTIFY {{Three times five is @result1}}
NOTIFY {{Ten times five is @result2}}
```

# Summary

*   `Function` is a powerful basic type in AlgoDraft that enables **first-class functions**, allowing them to be treated like data.
*   Its annotation syntax is used to specify the exact signature of a callable entity.
*   Shorthands exist for common cases, including **`Function<InputType -> ReturnType>`** for single-parameter functions and **`-> NULL`** for procedures.
*   For maximum flexibility in early design, **`AS Function`** can be used to denote any callable entity without specifying a signature.
*   Key uses include assigning functions to variables, passing functions as parameters to **higher-order functions** (for callbacks, predicates, etc.), and returning functions from other functions (**function factories**).
*   It is a good idea to use **`ALIAS`** to give a clear name to complex or frequently reused function types to improve the clarity of your algorithm designs.