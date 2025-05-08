---
status: Draft
---
In this chapter we are going to introduce ways to structure and reuse code by encapsulating logic into callable units. All involve defining a named block of code that can be called, potentially take inputs (**parameters**), and execute a sequence of steps. These are called **Functions**.

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
