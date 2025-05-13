---
status: Draft
---
In AlgoDraft, **Functions** are fundamental building blocks for creating structured and reusable algorithms. They allow you to encapsulate a specific piece of logic, a computation, or a series of actions into a named unit that can be executed (or "called") multiple times from different parts of your algorithm. This promotes modularity, readability, and makes your algorithms easier to understand, debug, and maintain.

A function in AlgoDraft can be designed to:

1. **Perform a specific task or action** (often called a "procedure"), such as printing output, modifying data, or interacting with an external entity. These functions typically do not return a direct computational result to the caller.

2. **Compute and return a single value** based on some input, much like mathematical functions.

3. **Communicate multiple pieces of information back to the caller**, either by returning a structured data type or by modifying variables provided by the caller through specially designated parameters.
# Declaration Syntax

The foundation of creating reusable logic in AlgoDraft is the function definition. This section outlines the precise syntax used to define a function, specifying its name, the parameters it accepts, the type of value it returns (if any), and the body of code that executes when the function is called.

A complete function definition in AlgoDraft follows this structure:

```
FUNCTION FunctionName (
    parameterDefinition_1,
    parameterDefinition_2,
    ...
    parameterDefinition_N
	) [-> ReturnType] :=
    // Function Body: Sequence of AlgoDraft statements
ENDFUNCTION
```

*   If a function takes no parameters, the parentheses `()` are still required but will be empty.

*   If a function does not return a value (i.e., it acts as a procedure), the `-> ReturnType` clause is omitted.

**Keywords and Clauses:**

*   **`FUNCTION`**: The keyword initiating a function definition.

*   **`FunctionName`**: The identifier (name) assigned to the function. This name is used to invoke the function. It should be conformed to PascalCasing.

*   **`(` ... `)` (Parentheses)**: Enclose the list of parameter definitions. Mandatory even if the list is empty.

*   **`parameterDefinition`**: Defines a single parameter the function accepts. The full syntax for a `parameterDefinition` is detailed in Function Parameters. Each definition typically includes:

    *   Parameter Directionality (`IN`, `OUT`, `INOUT`).
    
    *   Parameter Name (e.g., `$param1`).
    
    *   Parameter Data Type (e.g., `AS Integer`).
    
    *   Optional Default Value (e.g., `<- 0`).
    
    *   Multiple `parameterDefinition`s are separated by commas (`,`).

*   **`-> ReturnType` (Optional Return Clause)**:

    *   **`->`**: The arrow operator indicating a return value specification and delimits parameters list and return type..
    
    *   **`ReturnType`**: The data type of the value the function will return (e.g., `Integer`, `Boolean OR NULL`, `List<String>`). This clause is omitted for functions that do not return a value.

*   **`:=` (Definition Operator)**: Separates the function's signature (name, parameters, and return type) from its executable body. It signifies "is defined as."

*   **`Function Body`**: The block of AlgoDraft statements that implement the function's logic. If the function has a `-> ReturnType` clause, the body must use a `RETURN value` statement.

*   **`ENDFUNCTION`**: The keyword marking the termination of the function definition block.

**Example 1:** A function that returns a value:

```AlgoDraft
FUNCTION Add (
	IN $a AS Integer,
	IN $b AS Integer
	) -> Integer :=
	
	RETURN $a + $b
ENDFUNCTION
```

**Example 2:** A procedure (no return value) with an optional parameter:

```AlgoDraft
FUNCTION Greet (
	IN $name AS String,
	IN $greeting AS String <- "Hello"
	) := // No "-> ReturnType"
	
	$msg AS String <- $greeting + ", " + $name + "!"
	NOTIFY {{the greeting message of $msg}}
ENDFUNCTION
```

**Example 3:** A function with no parameters:

```AlgoDraft
FUNCTION GetCurrentTimestamp () -> DateTime :=
	$dateTime AS DateTime <- DO {{get current date and time of $tz time zone}}
	RETURN $dateTime
ENDFUNCTION
```
