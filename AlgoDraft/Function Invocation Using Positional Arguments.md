---
status: Planned
---
This is the simplest invocation method, mirroring mathematical notation. Arguments are matched to parameters based strictly on their order. The first argument in the call corresponds to the first parameter in the definition, the second argument to the second parameter, and so on.

**Syntax**
```
DoSomething(arg1, arg2, ...)
```

**Pros**:
* Conciseness
* familiar for individual with mathematical background

**Cons**:
* Requires remembering the exact parameter order.
* It can become unclear with many parameters or Boolean flags.
* It is difficult to skip optional arguments unless they are the last ones.

**Example 1**:
```
FUNCTION Calculate RETURNS Integer
	PARAMS
        IN $a AS Integer
        IN $b AS Integer
        IN $op AS String <- "+"
	ENDPARAMS
	
	// Body
ENDFUNCTION


$result AS Integer
$result <- Calculate(10, 5)      // $a=10, $b=5, $op uses default "+"
$result <- Calculate(20, 7, "*") // $a=20, $b=7, $op="*"
```

**Example 2**:
```
FUNCTION UpdateStatus
    PARAMS
        OUT $msg AS String
        IN $code AS Integer
    ENDPARAMS
	
	// Updating $msg in here...
ENDFUNCTION


$statusMessage AS String
UpdateStatus($statusMessage, 0) // $statusMessage is the variable argument for $msg, 0 for $code
                                // $statusMessage will be updated by the function
```

