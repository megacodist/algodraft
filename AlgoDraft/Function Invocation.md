---
status: Draft
---
Once you have defined a function, its logic is ready to be used. **Function invocation**, or calling a function, is the process of executing the code within a function's body. To call a function, you specify its name and provide the necessary data (as arguments) that the function's parameters expect.

The general syntax for calling a function is:

```
FunctionName(argument1, argument2, ...)
```

If the function is defined to return a value (using the `-> ReturnType` clause), you typically assign the result of the function call to a variable or use it in an expression:

```
$resultVariable AS ReturnType  
$resultVariable <- FunctionName(argument1, argument2, ...)
```

Let's explore the details of providing arguments and the different ways to invoke functions.

# Arguments: Supplying Data to Parameters

It's crucial to distinguish between parameters (defined in the function signature) and arguments (provided during the function call).

- **Parameter:** As discussed in Function Parameters, a parameter is a named placeholder declared within parentheses in function definition. It specifies the name, expected data type, and directionality (`IN`, `OUT`, `INOUT`) for data the function will work with.
	
	- In `FUNCTION Add(IN $num1 AS Integer, IN $num2 AS Integer) ...`, `$num1` and `$num2` are **parameters**.

- **Argument:** An argument is the actual value or variable that the caller provides when invoking the function. Arguments supply the concrete data that the parameters will represent during that specific function call.
	
	- In the call `Add(10, 5)`, the literal values `10` and `5` are the **arguments** corresponding to the `$num1` and `$num2` parameters, respectively.
        
    - Or suppose `FUNCTION Modify(INOUT $data AS MyRecord)` exists, in the call `Modify($myInstance)`, the variable `$myInstance` is the **argument** for the `$data` parameter.

**Analogy:** Parameters are like labeled input slots on a machine. Arguments are the actual items you put into those slots when you operate the machine.

## Supplying Arguments Based on Parameter Directionality:

The kind of argument you can (or must) provide depends on the Directionality of the corresponding parameter:

- **For `IN` Parameters:** The argument can be a literal value (e.g., `5`, `"hello"`), a variable, or any expression that evaluates to the expected `DataType`.

- **For `OUT` Parameters:** The argument **must** be a declared variable from the caller's scope. The function will write its output into this variable. You cannot pass a literal or anything that doesn't represent a modifiable storage location.

- **For `INOUT` Parameters:** Similar to `OUT`, the argument **must** be a declared variable from the caller's scope. The function will read the initial value from this variable and may write an updated value back into it.

There are three approaches for argument-parameter correspondence (sending arguments to parameters):

1. [[Function Invocation Using Positional Arguments]]
2. [[Function Invocation Using Named Arguments]]
3. [[Function Invocation Using Both Positional and Named Arguments]]

Whichever approach is being used, at least arguments must be provided for non-default-valued parameters.
