---
status: Planned
---
One of the powerful features of classes in AlgoDraft is **operator overloading**. This allows you to define custom behavior for standard AlgoDraft operators (like `+`, `-`, `=`, `NOT`, and even keyword operators like `IN` or custom word-like operators like `POW`) when they are applied to objects of your classes.

**What is Operator Overloading?**

Normally, operators have predefined meanings: `+` adds numbers, `AND` combines Booleans, etc. Operator overloading lets you extend these meanings, or define new ones, specifically for your custom class types.

For instance:

- You could define `+` for a `Vector2D` class to perform vector addition.

- You could define `=` for a `Book` class to compare if two books are identical based on their ISBN.

- You could define `IN` for a custom `DateRange` class to check if a `Date` falls within that range.

The primary goal is to make code that uses your objects more intuitive, readable, and "natural-feeling" by allowing the use of familiar operator symbols for class-specific actions. This is often referred to as providing "**syntax sugar**."

**General Syntax for Defining Operator Methods**

```
CLASS MyType
	OPERATOR <form_of_operands> [RETURNS ReturnDataType]
	ENDOPERATOR
ENDCLASS
```

In AlgoDraft, overloaded operators are defined as special instance methods within a class using the `OPERATOR` and `ENDOPERATOR` keywords.

- The "name" of the operator method is the **actual form of the operator as it would be used in code**, with `$this` representing the instance of your class involved in the operation.

- All overloaded operator methods must specify a `RETURNS ReturnDataType` clause if they produce a new value. If their primary purpose is to modify `$this` in-place and they don't logically produce a distinct result, the `RETURNS` clause is optional (similar to methods acting as procedures).

- Operator methods in AlgoDraft are **always `PUBLIC`**. This is because their main purpose is to define how objects of the class interact with external code using standard operator syntax.

- If an operator method needs to modify an operand, that operand **must be `$this`**. The other operand (for binary operators) is treated as input and is not modified by the operator method itself.

# Overloading Unary Operators

Unary operators act on a single operand, which must be `$this`. There are two forms:
1. Prefix
2. Postfix

## Unary Prefix Operators

In this syntax the operator symbol comes before `$this` like `NOT $this`, `- $this`.

**Syntax:**

```
OPERATOR <prefix_symbol> $this [RETURNS ReturnDataType]
    // Method body: logic using $this
    // ...
    [RETURN <resultValue>] // If RETURNS is specified
ENDOPERATOR
```

**Example:**

In the following example, we are going to overload `NOT` for a `Switch` class.

```
CLASS Switch
    PRIVATE $isOn AS Boolean
	
    METHOD CONSTRUCTOR
        PARAMS
	        $initialState AS Boolean <- FALSE
        ENDPARAMS
        
        $this.$isOn <- $initialState
    ENDMETHOD
	
    // Overload the NOT operator to toggle the switch's state
    OPERATOR NOT $this RETURNS Switch // Returns a new Switch with the toggled state
        RETURN NEW Switch(NOT $this.$isOn)
    ENDOPERATOR

    // A method to display status
    METHOD DisplayStatus
        IF $this.$isOn THEN
            NOTIFY {{the switch is on}}
        ELSE
            NOTIFY {{the switch is off}}
        ENDIF
    ENDMETHOD
ENDCLASS

$lightSwitch AS Switch <- NEW Switch(FALSE)
$lightSwitch.DisplayStatus() // the user will be informed switch is off

$toggledState AS Switch
$toggledState <- NOT $lightSwitch // Uses the overloaded NOT operator
$toggledState.DisplayStatus() // the user will be informed switch is on
$lightSwitch.DisplayStatus()  // the user will be informed switch is off
```

## Unary Postfix Operators

In this form of operator overloading, the operator symbol comes after `$this` e.g., $this++, $this--.

**Syntax**:

```
OPERATOR $this <PostfixSymbol> [RETURNS ReturnDataType]
    // Method body: logic that usually modifies $this
    // The return value often depends on convention (value before or after modification)
    // ...
    [RETURN <resultValue>] // If RETURNS is specified
ENDOPERATOR
```

**Example**:

It can be possible to overload `++` (postfix increment) for a `ScoreCounter` class (modifies in place, returns old value):

```
CLASS ScoreCounter
    PRIVATE $value AS Integer
	
    METHOD CONSTRUCTOR
        PARAMS
	        $initialValue AS Integer <- 0
        ENDPARAMS
        
        $this.$value <- $initialValue
    ENDMETHOD

    // Overload postfix ++ to increment value and return the value BEFORE incrementing
    OPERATOR $this ++ RETURNS Integer
        $oldValue AS Integer <- $this.$value
        $this.$value <- $this.$value + 1
        RETURN $oldValue
    ENDOPERATOR

    METHOD GetValue RETURNS Integer
        RETURN $this.$value
    ENDMETHOD
ENDCLASS

$playerScore AS ScoreCounter <- NEW ScoreCounter(10)
$previousScore AS Integer

$previousScore <- $playerScore ++ // Uses overloaded ++
NOTIFY {{the previous and current values are $previousScore and $playerScore.GetValue() respectively}}
```

# Binary Operators

Binary operators act on two operands. In an overloaded operator method, `$this` usually represents the left-hand operand (`$this ^= $other`), although it depends on the context (`$other PREPENDS TO $this`). The other operand is defined in an `OPERANDS` block.

**Syntax**:

```
OPERATOR <binary_operator_form> [RETURNS ReturnDataType]
    OPERANDS
        $other AS OtherOperandDataType // Definition of the right-hand operand
    ENDOPERANDS
	
    // Method body: logic using $this (left) and $other (right)
    // ...
    [RETURN <resultValue>] // If RETURNS is specified
ENDOPERATOR
```

* **`<binary_operator_form>`:** the form of binary operator containing `$this` and `$other` like `$this ^= $other` or `$other PREPENDS TO $this`.

- **`$other AS OtherOperandDataType`**: Defines the other operand. This parameter is treated as input and is not modified by the operator method.

**Example**:

In the next example, the `+` operator is overloaded a `Vector2D` class.

```
CLASS Vector2D
    PUBLIC $x AS Real
    PUBLIC $y AS Real

    METHOD CONSTRUCTOR
        PARAMS
	        $xVal AS Real
	        $yVal AS Real
        ENDPARAMS
        $this.$x <- $xVal
        $this.$y <- $yVal
    ENDMETHOD

    // Overload + to perform vector addition
    OPERATOR $this + $otherVec RETURNS Vector2D
        OPERANDS
            $otherVec AS Vector2D
        ENDOPERANDS
        $resultX AS Real <- $this.$x + $otherVec.$x
        $resultY AS Real <- $this.$y + $otherVec.$y
        $newVec AS Vector2D <- NEW Vector2D($resultX, $resultY)
        RETURN $newVec
    ENDOPERATOR

    // Example of overloading a keyword operator
    OPERATOR $this EQUALS $otherVec RETURNS Boolean // Using a word for '='
        OPERANDS
            $otherVec AS Vector2D
        ENDOPERANDS
        RETURN ($this.$x = $otherVec.$x) AND ($this.$y = $otherVec.$y)
    ENDOPERATOR
ENDCLASS

$v1 AS Vector2D <- NEW Vector2D(1.0, 2.0)
$v2 AS Vector2D <- NEW Vector2D(3.0, 4.0)
$v3 AS Vector2D

$v3 <- $v1 + $v2 // Uses overloaded +
NOTIFY {{$v3 is the addition of $v1 and $v2}}

$v4 AS Vector2D <- NEW Vector2D(1.0, 2.0)
IF $v1 EQUALS $v4 THEN // Uses overloaded EQUALS
    NOTIFY {{v1 and v4 are equal.}}
ENDIF
```

**Example:**

A `Wallet` class can be overloaded with the in-place addition `+=`.

```
CLASS Wallet
    PRIVATE $balance AS Real
	
    METHOD CONSTRUCTOR
        PARAMS
	        $initialAmount AS Real <- 0.0
        ENDPARAMS
        $this.$balance <- $initialAmount
    ENDMETHOD
	
    // Overload += to add to balance, modifies $this, no return value needed from expression
    OPERATOR $this += $amountToAdd
        OPERANDS
            $amountToAdd AS Real
        ENDOPERANDS
        IF $amountToAdd > 0.0 THEN
            $this.$balance <- $this.$balance + $amountToAdd
        ENDIF
    ENDOPERATOR
	
    METHOD GetBalance RETURNS Real
        RETURN $this.$balance
    ENDMETHOD
ENDCLASS


$myWallet AS Wallet <- NEW Wallet(50.0)
$myWallet += 20.0 // Uses overloaded +=
NOTIFY {{new balance is $myWallet.GetBalance()}}
```

# The "Form" of an Overloadable Operator

AlgoDraft offers a flexible ("loose") syntax for the operator form you can define. The `<form_of_oerator>` in the syntax can be:

1. **Non-alphanumeric characters:** Standard operator symbols like +, -, *, /, %, =, !=, <, <=, >, >=, !, &&, ||, ++, --, **.

2. **Existing keyword operators:** Keywords that act like operators, such as `IN`, `ITERATOR OF`, `SUBSET OF`.

```
// Example for a CustomSet class
OPERATOR $element IN $this RETURNS Boolean
    // ... logic to check if $element is in the set represented by $this ...
ENDOPERATOR
```

3. **Non-keyword, all-uppercase alphanumeric words:** Custom named operators for domain-specific clarity.

```
// Example for a Matrix class
OPERATOR $this MULTIPLY $otherMatrix RETURNS Matrix
    OPERANDS
	    $otherMatrix AS Matrix
	ENDOPERANDS
    // ... matrix multiplication logic ...
ENDOPERATOR
```

**Important Syntactic Restrictions for Operator Forms:**

- The form **must not** use non-operator AlgoDraft keywords (like `CLASS`, `IF`, `WHILE`, `METHOD`, `PARAMS`).

- The form **must not** use combinations of characters that are already syntactically significant in a conflicting way (e.g., `//` for comments, `<-` for assignment, `.` for member access, `AS` for type declaration).

- One of the operands in the operator's usage form **must be `$this`** in the definition.

**A Word of Caution: Responsibility and Readability**

The flexibility of operator overloading in AlgoDraft is powerful, but with great power comes great responsibility. **PLEASE DO NOT OVERWHELM YOURSELF AND OTHER POOR CODERS!**

- **Strive for Intuition:** Overload operators in a way that aligns with their common mathematical or logical meaning, or is exceptionally clear within a specific domain. The goal is to enhance readability, not obscure it.

- **Clarity Over Cleverness:** If an operation is complex or its meaning isn't immediately obvious with an operator symbol, a well-named regular method is often a better choice.

- **The Principle of Least Surprise:** Operators should behave in a way that users would generally expect. Avoid making `+` perform subtraction, for instance.

- **Use Parentheses Liberally:** AlgoDraft does not define precedence rules for newly invented operator forms (like `MULTIPLY` or custom symbols) relative to each other or to all built-in operators. When using multiple overloaded operators in an expression, or mixing them with standard operators, **use parentheses `()` extensively** to explicitly control the order of evaluation and ensure your logic is unambiguous and clear to all readers.

Operator overloading, when used thoughtfully, can make your AlgoDraft classes more expressive and your algorithms more elegant.
