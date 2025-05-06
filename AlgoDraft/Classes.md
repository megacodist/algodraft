---
status: Planned
---
In AlgoDraft, you've already learned about **Records**, which are excellent for grouping related data fields together. Now, we'll introduce **Classes**. You can think of a **Class as a more powerful version of a Record: it not only holds data (called attributes, just like fields in Records) but also defines operations that can be performed on that data.**

While Records are primarily passive data containers, Classes allow you to create blueprints for 'active' objects that have both state (their data) and behavior (what they can do via their methods and/or operators).



**9.1 Introduction to Classes: Why We Need More Than Records**
    *   Recap: What Records are good for (data aggregation).
    *   The Limitation: Records are passive data containers. What if we want to bundle data *with the operations* that logically belong to that data?
    *   Introducing Classes: Blueprints for objects that combine state (attributes) and behavior (methods).
    *   Analogy: "Classes are like Records, but with built-in functions (methods) that know how to work with their own data."

**9.2 Defining a Class**
    *   Syntax: `CLASS ClassName ... ENDCLASS` (ClassName is `PascalCase`).
    *   **9.2.1 Defining Attributes (Fields)**
        *   Syntax is identical to defining fields in Records:
            ```pseudocode
            CLASS MyClass
                // Attributes without defaults (must be initialized by constructor)
                $attributeA AS DataTypeA
                // Attributes with defaults
                $attributeB AS DataTypeB <- <defaultValueB>
            ENDCLASS
            ```
        *   Reiterate the ordering rule (no-defaults first, then defaults).
    *   **9.2.2 Defining Methods (Functions within a Class)**
        *   Syntax: `FUNCTION MethodName($param1 AS Type, ...) AS ReturnType ... ENDFUNCTION` (same as standalone functions).
        *   Placement: Defined *inside* the `CLASS ... ENDCLASS` block.
        *   **The `SELF` Keyword:**
            *   Introducing `SELF` (or `THIS` - be consistent).
            *   Purpose: How methods refer to the specific instance of the class they are operating on.
            *   Accessing attributes: `SELF.$attributeName`.
            *   Calling other methods of the same instance: `SELF.OtherMethodName(...)`.
        *   Example of a simple class with attributes and a method.
    *   **9.2.3 The Constructor Method (Special Method for Initialization)**
        *   Purpose: A special method automatically called when `NEW ClassName(...)` is used.
        *   Syntax: How to define a constructor (e.g., `FUNCTION CONSTRUCTOR($param1, ...)`, or `FUNCTION ClassName(...)` - choose one and be consistent).
        *   Role: Initialize attributes, especially those without default values defined directly in the attribute list.
        *   Relationship to `NEW`: Arguments passed to `NEW ClassName({{...}})` are typically passed to the constructor.
        *   How it differs from Record instantiation: Classes have explicit constructor logic, Records have implicit initialization based on `NEW` arguments and defaults.

**9.3 Creating Class Instances (Objects)**
    *   Using `NEW ClassName({{arguments}})`:
        *   Reiterate that this invokes the constructor.
        *   Argument passing rules (`{{...}}` with positional/named/mixed) are the same as function calls and record instantiation, now mapping to constructor parameters.
        *   Example: `$myObject AS MyClassName <- NEW MyClassName({{arg1, arg2, namedArg: value}})`

**9.4 Accessing Attributes and Calling Methods**
    *   Accessing Attributes: `$objectInstance.$attributeName` (for reading and writing, same as records).
    *   Calling Methods: `$objectInstance.MethodName({{arguments}})` (again, `{{...}}` for arguments if the method takes any).
    *   Example showing attribute access and method calls.

**9.5 Example: A Simple Counter Class**
    *   Define a `Counter` class with an attribute `$count` and methods like `Increment`, `Decrement`, `GetValue`.
    *   Show instantiation and usage.

**9.6 (Optional/Advanced) Operator Overloading within Classes**
    *   Introduction: What is operator overloading? (Allowing standard operators like `+`, `==`, `<` to have custom behavior for class instances).
    *   Syntax: How to define operator methods (e.g., `OPERATOR + ($other AS MyClass) AS MyClass`, `OPERATOR == ($other AS MyClass) AS Boolean`).
    *   Connection to Interfaces: How this might relate to `IEquatable`, `IComparable` (a class can implement these by defining the corresponding operator methods).
    *   Simple example (e.g., a `Vector2D` class overloading `+` for vector addition).
    *   *Keep this section high-level for pseudocode, focusing on the concept rather than deep implementation details.*

**9.7 Summary: Classes vs. Records**
    *   Quick table or bullet points highlighting key differences and use cases.
        *   Records: Primarily for data grouping, simpler instantiation (no explicit constructor method definition needed from the user).
        *   Classes: Data + Behavior (methods), explicit constructor for controlled initialization, can support operator overloading, form the basis for more advanced OO patterns.