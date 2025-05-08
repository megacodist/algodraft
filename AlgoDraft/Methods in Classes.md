---
status: Draft
---
While attributes define the state (data) of an object, **methods** define its behavior (what it can do). Methods are essentially functions that are bound to a class and operate on the data of a specific instance of that class (or on class-level data, in the case of static methods).

The syntax for defining a method in AlgoDraft is very similar to defining a standalone [[Functions|function]], with a few key distinctions that tie methods to their classes and objects.

# Syntax

```
[<visibility>] [STATIC] METHOD DoSomething [RETURNS ReturnDataType]
    PARAMS
        // Parameter declarations (optional)
        // Same syntax as function parameters, including modes and default values
        [IN | OUT | INOUT] $param1 AS DataType1
        [IN | OUT | INOUT] $param2 AS DataType2 <- <defaultValue2>
        // ...
    ENDPARAMS

    // Method Body: Contains AlgoDraft statements
    // ...
    // Use '$this' to access instance attributes/methods (for non-static methods)
    // Use 'ClassName.$staticAttribute' to access static attributes
    // ...
    [RETURN $resultValue] // If the method has a RETURNS clause
ENDMETHOD
```

Let's explore the key aspects and differences compared to standalone functions:

## `METHOD ... ENDMETHOD` Keywords

The most direct syntactic difference is the use of keywords:

- Methods within a class are defined using **`METHOD`** to start the definition and **`ENDMETHOD`** to end it.

- Standalone functions use `FUNCTION` and `ENDFUNCTION`.

This clearly distinguishes a callable block of code that belongs to a class from one that is independent.

```
CLASS ExampleClass
    METHOD MyInstanceMethod // This is a method
        NOTIFY {{control low is inside MyInstanceMethod}}
    ENDMETHOD
ENDCLASS

FUNCTION MyStandaloneFunction // This is a standalone function
    NOTIFY {{control flow is inside MyStandaloneFunction}}
ENDFUNCTION
```

## The `$this` Variable: Referring to the Current Instance

For **non-static** methods (the most common type), AlgoDraft provides a special variable: **`$this`**.

- **Purpose:** Inside a non-static method, `$this` refers to the **specific object instance** on which the method was called. It's how a method accesses its own object's attributes and calls other methods of that same object.

- **Implicit:** You do not declare `$this` as a parameter; it's implicitly available.

- **Usage:**
    
    - To access an instance attribute: `$this.$attributeName`
    
    - To call another non-static method of the same object: `$this.OtherMethodName({{arguments here}})`
    
    - To call a static method of the same class: `ClassName.StaticMethod(.{{arguments here}})`.

**Example**:

```AlgoDraft
CLASS Greeter
    PRIVATE $greetingMessage AS String
	
    METHOD CONSTRUCTOR
        PARAMS
	        $message AS String
        ENDPARAMS
        
        $this.$greetingMessage <- $message // Use $this to set the instance's attribute
    ENDMETHOD

    PUBLIC METHOD SayHello
        PARAMS
	        $name AS String
        ENDPARAMS
        
        // Use $this to access the instance's $greetingMessage
        $msg AS String <- $this.$greetingMessage + ", " + name + "!"
        NOTIFY {{$msg as greeting}}
        $this.LogGreeting() // Call another method of this instance
    ENDMETHOD

    PRIVATE METHOD LogGreeting
	    $msg AS String <- "Greeting logged for message: " + $this.$greetingMessage
        DO {{log $msg}}
    ENDMETHOD
ENDCLASS

$formalGreeter AS Greeter <- NEW Greeter("Good day")
$casualGreeter AS Greeter <- NEW Greeter("Hey there")

$formalGreeter.SayHello("Ms. Smith") // $this inside SayHello refers to $formalGreeter
$casualGreeter.SayHello("Bob")   // $this inside SayHello refers to $casualGreeter
```

Standalone functions do not have `$this` because they are not associated with a specific object instance in the same way.

## Constructors: Special Initialization Methods

**`METHOD CONSTRUCTOR`** is a special method unique to classes.

- **Purpose:** It's automatically called when a new object of the class is created using `NEW ClassName({{arguments here}})`. Its sole responsibility is to initialize the instance attributes of the new object.

- **Key Differences from Regular Methods:**
    
    - Fixed name: `CONSTRUCTOR`.
    
    - No `RETURNS` clause (it implicitly returns the new object instance).
    
    - Parameters do not use directionality (`IN`/`OUT`/`INOUT`); they are for input to set initial state.

Standalone functions do not have this special constructor role.

## Static Methods

Just as classes can have static attributes (belonging to the class itself), they can also have **static methods**.

- **Declaration:** Declared using the **`STATIC`** keyword before `METHOD`.

```
PUBLIC STATIC METHOD UtilityFunction RETURNS Boolean
    PARAMS
	    $param AS SomeType
    ENDPARAMS
    // ...
ENDMETHOD
```

- **No `$this`:** Static methods are **not** associated with any particular instance of the class. Therefore, they **do not receive an implicit `$this`** reference and cannot directly access instance attributes (`$this.$attributeName`).

- **Access:** They are called using the class name itself, not an object instance: `ClassName.UtilityFunction({{arguments here}})`.

- **Purpose:** Often used for utility functions that are logically related to the class but don't require access to instance-specific data. They can operate on static attributes of the class or on the parameters passed to them.

**Example of a Static Method:**

```
CLASS MathHelper
    PUBLIC STATIC CONST $PI AS Real <- 3.14159
	
    // Static method: Calculates area of a circle
    PUBLIC STATIC METHOD CalculateCircleArea RETURNS Real
        PARAMS
	        $radius AS Real
        ENDPARAMS
        // Can access static attributes like MathHelper.$PI
        // CANNOT access $this or any instance attributes
        RETURN MathHelper.$PI * $radius * $radius
    ENDMETHOD
	
    // Instance method (example, though less common for a pure utility class)
    PUBLIC $lastResult AS Real
    
    METHOD CONSTRUCTOR
        $this.$lastResult <- 0.0
    ENDMETHOD
ENDCLASS

$area AS Real
$area <- MathHelper.CalculateCircleArea(5.0) // Calling static method
NOTIFY "Area is: " & $area

// $instance AS MathHelper <- NEW MathHelper()
// $instance.CalculateCircleArea(2.0) // Typically not how static methods are called
                                     // though some languages allow it, it's clearer via class name.
```

Standalone functions are always "static" in the sense that they don't have an instance context, but static methods are explicitly part of a class's namespace and structure.

Beyond these differences, methods share many syntactic similarities with standalone functions:

- **Optional `RETURNS ReturnDataType` clause:** If present, the method must use a `RETURN $value` statement. If absent, it's a procedure-like method.

- **`PARAMS ... ENDPARAMS` block:** For defining parameters, including their mode (`IN`, `OUT`, `INOUT` - except for `CONSTRUCTOR`) and optional default values (`<- <defaultValue>`).

- **Method Body:** Contains the sequence of AlgoDraft statements that define the method's logic.

In essence, methods are functions that "live inside" classes, gaining special access to object instance data via `$this` variable or operating at the class level if `STATIC`.