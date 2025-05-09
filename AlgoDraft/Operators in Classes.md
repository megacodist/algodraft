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

## Binary Operators

Binary operators act on two operands. In an overloaded operator method, `$this` represents the left-hand operand. The right-hand operand is defined in an `OPERANDS` block.

**Syntax**:

```
OPERATOR $this <OperatorSymbolOrWord> $otherParameterName [RETURNS ReturnDataType]
    OPERANDS
        $otherParameterName AS OtherOperandDataType // Definition of the right-hand operand
    ENDOPERANDS
	
    // Method body: logic using $this (left) and $otherParameterName (right)
    // ...
    [RETURN <resultValue>] // If RETURNS is specified
ENDOPERATOR
```

- **`$otherParameterName AS OtherOperandDataType`**: Defines the right-hand operand. This parameter is treated as input and is not modified by the operator method.

