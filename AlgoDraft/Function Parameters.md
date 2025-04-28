---
status: Draft
---
One or more parameters can be declared using the following syntax. If function does not take any parameter, the whole `PARAMS`/`ENDPARAMS` block must be omitted.

**Syntax**:
```
[IN | OUT | INOUT] $paramName AS DataType [<- <defaultValue>]
```

* `[IN | OUT | INOUT]`: The optional [[#Parameter Directionality (`IN`, `OUT`, `INOUT`)|Parameter Directionality]], defaults to `IN`.
* `$paramName`: The identifier (name) used to refer to this parameter inside the body of the routine.
* `AS DataType` (mandatory): Declares the expected data type for the parameter.
* `[<- <defaultValue>]`: The optional default value.

# Default-Valued Parameters
In the function definition, you can make a parameter optional for the caller by providing a default value using the `<- defaultValue` syntax.

```
FUNCTION DoSomething RETURNS SomeType
    PARAMS
        $param1 AS Integer
        $param2 AS Integer <- <defaultValue>
    ENDPARAMS
    
    // Doing something...
ENDFUNCTION
```

* If default value **present**, this argument becomes optional for the caller. If the caller doesn't provide a value/argument for this parameter, it will automatically be initialized with the `defaultValue`.

* Arguments with default values must come after arguments without default values in the list.

**Example**:
```
FUNCTION Connect
   PARAMS
      IN $host AS String           // Required parameter
      IN $port AS Integer <- 8080  // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION
```

If the caller does not provide an argument corresponding to `$port` or `$timeout` when calling Connect, the function will automatically use `8080` for `$port` and `5000` for `$timeout` internally.

# Parameter Directionality (`IN`, `OUT`, `INOUT`)
When defining an argument within the `PARAMS`/`ENDPARAMS` block, you can specify its **Parameter Directionality**. This is an optional keyword (`IN`, `OUT`, or `INOUT`) that declares the intended direction of data flow for that parameter between the caller's argument and the routine's parameter. It defaults to `IN`.

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
