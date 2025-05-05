---
status: Draft
---
# Arguments

The basics of function invocation syntax is inspired by mathematics. To call a function; we must put a comma-separated list of literals, variables, or anything that is evaluated to a value; in a pair of parentheses just in front of function name. These are called **arguments**.

**Example**
```
FUNCTION Connect
   PARAMS
      IN $host AS String           // Required parameter
      IN $port AS Integer <- 8080  // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION


Connect("example.com")
Connect("example.com", 443)
Connect("example.com", 443, 1000)
```

**Parameters vs. Arguments**
These terms are related but distinct:

* **Parameter**: A variable declared within the `PARAMS`/`ENDPARAMS` block of a function definition. It's a placeholder defining the name, expected data type, and directionality (`IN`, `OUT`, `INOUT`) of an expected input or output. In the above example `$host`, `$port`, and `$timeout` are parameters.

* **Argument**: The actual value (literal or evaluation of an expression) or variable supplied by the caller when the function is invoked. Arguments provide the concrete data that the parameters will hold during the function's execution. In the above example `"example.com"`, `443`, and `1000` are arguments.

There are three approaches for argument-parameter correspondence (sending arguments to parameters):

1. [[Function Invocation Using Positional Arguments]]
2. [[Function Invocation Using Named Arguments]]
3. [[Function Invocation Using Both Positional and Named Arguments]]

Whichever approach is being used, at least arguments must be provided for non-default-valued parameters.