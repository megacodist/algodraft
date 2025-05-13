---
status: Planned
---
This is the most traditional way to call functions, mirroring mathematical notation, where arguments are matched to parameters based strictly on their order. The first argument in the function call is assigned to the first parameter defined in the function's signature, the second argument to the second parameter, and so on.

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

* It is difficult to skip optional arguments unless one or more `IN`, default-valued parameters from the last.

**Example 1: Simple `IN` parameters:**

```
FUNCTION DescribePerson($name AS String, $age AS Integer) :=
	$msg AS String <- $name + " is " + $age + " years old."
	NOTIFY {{$msg}}
ENDFUNCTION

// Calling with positional arguments:
DescribePerson("Alice", 30) // $name becomes "Alice", $age becomes 30
// Output: Alice is 30 years old.
```

**Example 2: Handling `OUT` and `INOUT` parameters:**

```
FUNCTION SwapValues(INOUT $val1 AS Integer, INOUT $val2 AS Integer) :=
	$temp AS Integer
	$temp <- $val1
	$val1 <- $val2
	$val2 <- $temp
ENDFUNCTION

$a AS Integer <- 10
$b AS Integer <- 20
NOTIFY {{initial values before swapping are $a and $b}}
SwapValues($a, $b) // $a is argument for $val1, $b for $val2
NOTIFY {{final values after swapping are $a and $b}}
```

**Example 3: Using with default-valued parameters:**  

If a function has parameters with default values, you can omit trailing arguments, and the defaults will be used.

```
FUNCTION Greet($name AS String, $message AS String <- "Hello") :=
	$msg AS String <- $message + ", " + $name + "!"
    NOTIFY {{$msg}}
ENDFUNCTION

Greet("Bob") // $name is "Bob", $message uses default "Hello"
// The user is notified "Hello, Bob!"
Greet("Charlie", "Good morning") // Both arguments provided
// The user is notified "Good morning, Charlie!"
```

**Limitation:** With purely positional arguments, you cannot easily skip an optional parameter in the middle of the list while providing one that comes after it, unless the language has a specific syntax for "empty" arguments (which AlgoDraft does not explicitly define here).
