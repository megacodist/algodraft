---
status: Draft
---
In AlgoDraft, you've already become familiar with **Records**. Records are an excellent tool for grouping related data fields into a single, structured unit. For example, a `Point2D` record might hold an `$xCoord` and a `$yCoord`, or a `UserProfile` record could bundle `$userName`, `$userId`, and `$email`. This helps organize data and make it more manageable than handling many separate variables.

However, Records are primarily **passive data containers**. They hold information, but they don't inherently know what to do with that information or how to perform operations related to it. If you wanted to calculate the distance of a `Point2D` from the origin, or check if a `UserProfile`'s email is valid, you would typically write separate, standalone functions that take a record instance as input:

```AlgoDraft
// Assuming RECORD Point2D and FUNCTION CalculateDistance defined elsewhere
$p1 AS Point2D <- NEW Point2D(xCoord <- 3.0, yCoord <- 4.0)
$dist AS Real
$dist <- CalculateDistance($p1) // Standalone function acts on the record data
```

While this works, as our systems and data become more complex, we often find that certain pieces of data and the operations that logically belong to that data are tightly coupled. For instance, the logic to Increment or Reset a counter naturally belongs with the counter's current value.

This is where **Classes** come in. You can think of a **Class as a more powerful and comprehensive version of a Record**. A Class serves as a blueprint for creating "objects" that not only hold **data** (called **attributes**, just like fields in Records) but also define **operations** (called **methods**, which are essentially functions bound to the class) that can be performed on that specific data.

**Key Idea:** Classes allow you to bundle data and the behavior that acts on that data together into a single, cohesive unit.

| Feature         | Records                                  | Classes                                                                      |
| --------------- | ---------------------------------------- | ---------------------------------------------------------------------------- |
| **Primary Use** | Grouping related data fields.            | Grouping data (attributes) **and** related behavior (methods and operators). |
| **Nature**      | Passive data containers.                 | Active objects with state and defined operations.                            |
| **Operations**  | Typically handled by external functions. | Often built-in as methods directly associated with the data.                 |

By using Classes, we can create more organized, modular, and intuitive designs, especially for more complex entities in our algorithms. Instead of having loose data and separate functions, we create self-contained objects that "know" how to manage themselves and interact with the world.

In the following sections, we'll explore how to define classes, create objects from them, and use their attributes and methods in AlgoDraft.

# Attributes and the Construction

A **Class** in AlgoDraft is a blueprint for creating objects. It defines the **attributes** (data) an object will hold and the **methods and operators** (behavior) it can perform. We'll first focus on the overall structure, attributes, and a special method called the `CONSTRUCTOR` which is responsible for initializing new objects.

**Overall Class Definition Syntax:**

```AlgoDraft
CLASS ClassName
	
    // --- Attributes Section ---
    <attribute_definition_1>
    <attribute_definition_2>
    // ...
	
    // --- Methods Section ---
    <method_definition_1> // e.g., METHOD CONSTRUCTOR ... ENDMETHOD
    <method_definition_2> // e.g., METHOD DoSomething ... ENDMETHOD
    // ...
	
    // --- (Optional) Operators Section ---
    <operator_definition_1>
    <operator_definition_2>
    // ...
	
ENDCLASS
```

**Keywords and Clauses:**

1. **`CLASS ClassName ... ENDCLASS`**:
    
    - The `CLASS` keyword initiates the definition, followed by the chosen `ClassName` (which must be conformed to PascalCase).
    
    - The `ENDCLASS` keyword concludes the definition. All attributes, methods, and operators definitions for this class must reside between these keywords.

2. **Attributes Section**:
    
    - This is where you declare the variables that will store the data associated with the class and its objects.
    
    - A class typically has **one or more attributes** to hold its state.
        
    - AlgoDraft distinguishes between:
        
        - **Class Attributes (Static Attributes):** Shared by all instances of the class.
            
        - **Instance Attributes:** Unique to each object created from the class.
            

3. **Methods Section**:
    
    - This section contains the definitions of functions that are bound to the class. These are called **methods**.
    
    - Methods implement the behavior of the class's objects.
    
    - A class **must have at least one method**, which is the special `CONSTRUCTOR` method responsible for initializing new objects.
    
    - If a type only needs to group data and doesn't require any specific behavior (i.e., no methods other than potentially a very simple constructor that could be handled by record instantiation rules), it is **highly recommended to re-define it as a RECORD instead of a CLASS** for simplicity. Classes are intended for types that combine data with behavior.
    
4. **Operators Section (Optional)**:
    
    - AlgoDraft allows classes to define custom behavior for standard operators (e.g., +, -, =, <). This feature is called **operator overloading**.
    
    - This section is optional; many classes will not need to overload operators.
    
    - The reasonable use of operator overloading can increase code readability.

5. **Logical, Not Syntactically Enforced, Sections**:
    
    - The division into "Attributes Section," "Methods Section," and "Operators Section" as shown above (often separated by comments) is a **strong convention for readability and organization**.
    
    - However, AlgoDraft does **not** syntactically enforce these specific section demarcations or their order with special keywords (beyond the overall `CLASS ... ENDCLASS` block). You could, for example, interleave attribute and method definitions if you chose, though this is generally discouraged for clarity. The primary requirement is that attributes are declared before they are used (e.g., by methods within the same class). Adhering to the grouped structure is highly recommended for maintainable pseudocode.

This general structure provides the framework. In the following subsections, we will delve into the specific syntax and rules for defining attributes, understanding visibility, crafting methods (including the constructor), and optionally overloading operators.






Access syntax for class and instance attributes and methods.






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