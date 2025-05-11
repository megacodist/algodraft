---
status: Draft
---
- **x.1. Function Syntax:** We will start by detailing the precise syntax for defining a function, including how to name it, specify its inputs, indicate what (if anything) it returns, and write its body of executable statements.
    
- **x.2. Function Parameters:** Functions often need input data to work with. This subsection explains how to define parameters, which act as placeholders for the data a function will receive when called. We'll cover:
    
    - **x.2.1. Default-Valued Parameters:** How to make parameters optional by providing default values.
        
    - **x.2.2. Parameter Directionality:** A crucial concept detailing how data flows between the caller and the function using IN, OUT, and INOUT modes, defining the contract for each parameter.
        
- **x.3. Function Invocation:** Once a function is defined, you need to know how to call it. This subsection covers the syntax and rules for function invocation, including:
    
    - **x.3.1. Arguments:** Understanding the difference between parameters (in the definition) and arguments (the actual values/variables passed during a call).
        
    - **x.3.2. Function Invocation Using Positional Arguments:** The common method of providing arguments based on their order.
        
    - **x.3.3. Function Invocation Using Named Arguments:** A more explicit and often clearer way to provide arguments by matching them to parameter names.
        
    - **x.3.4. Function Invocation Using Both Positional and Named Arguments:** Combining both methods for flexibility.

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
    // Parameter list (see x.2. Function Parameters for details)
    [ParameterDirectionality] $parameterName1 AS DataType1 [<- defaultValue1],
    [ParameterDirectionality] $parameterName2 AS DataType2 [<- defaultValue2],
    ...
	) [-> ReturnType] :=
    // Definition operator: separates signature from body
    //
    // Function Body:
    // Sequence of AlgoDraft statements that implement the function's logic.
    // This is where computations are performed and actions are taken.
    // May include 'RETURN' statements.
    //
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
