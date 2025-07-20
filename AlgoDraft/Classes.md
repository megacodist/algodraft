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
CLASS ClassName [INHERITS BaseClass] [IMPLEMENTS Interface1, interface2, ...]:=
	
    // --- Attributes Section ---
    <attribute_definition_1>
    <attribute_definition_2>
    // ...
    
	// --- (Optional) Functions Section ---
    <function_definition_1> // e.g., FUNCTION FromSomethingElse ... ENDFUNCTION
    <function_definition_2> // e.g., FUNCTION GetThatUniversal ... ENDFUNCTION
    // ...
	
    // --- Methods Section ---
    <method_definition_1> // e.g., METHOD NEW ... ENDMETHOD
    <method_definition_2> // e.g., METHOD DoSomething ... ENDMETHOD
    // ...
	
    // --- (Optional) Operators Section ---
    <operator_definition_1>
    <operator_definition_2>
    // ...

	// --- (Optional) NLIs Section ---
    <nli_definition_1>  // e.g. DO {{Natural language description}}
    <nli_definition_2>
    // ...
	
ENDCLASS
```

**Keywords and Clauses:**

1. **`CLASS ClassName ... ENDCLASS`**:
    
    - The `CLASS` keyword initiates the definition, followed by the chosen `ClassName` (which must be conformed to PascalCase).
    
    - The `ENDCLASS` keyword concludes the definition. All attributes and operations definitions for this class must reside between these keywords.

2. **Attributes Section**:
    
    - This is where you declare the variables that will store the data associated with the class and its objects.
    
    - A class typically has **one or more attributes** to hold its state.
        
    - AlgoDraft distinguishes between:
        
        - **Class Attributes (Static Attributes):** Shared by all instances of the class.
            
        - **Instance Attributes:** Unique to each object created from the class.
            

3. **Functions Section (Optional)**:
    
    - This section contains the definitions of **class functions**, which are utility functions logically grouped with a class but **not** tied to a specific object instance. They are analogous to "static methods" in other languages.
    
    - Class functions **do not** have access to the this variable and are called directly on the class itself (e.g., ClassName.FunctionName(...)). They are often used for factory functions or utility logic related to the class concept.

4. **Methods Section**:
    
    - This section contains the definitions of functions that are bound to the class. These are called **methods**.
    
    - Methods implement the behavior of the class's objects.
    
    - A class **must have at least one method**, which is the special `NEW` method responsible for initializing new objects.
    
    - A class can be used to simply group data if it only contains attributes and a basic constructor. However, the true power of classes is realized when they combine data with a rich set of behaviors.
    
5. **Operators Section (Optional)**:
    
    - AlgoDraft allows classes to define custom behavior for standard operators (e.g., +, -, =, <). This feature is called **operator overloading**.
    
    - This section is optional; many classes will not need to overload operators.
    
    - The reasonable use of operator overloading can increase code readability.

6. **NLIs Section (Optional)**:
    
    - This section contains definitions for **Natural Language Instructions (NLIs)**. These are a unique AlgoDraft feature for defining high-level, descriptive operations on an instance using a natural language phrase.
    
    - An NLI is defined using the `DO` keyword followed by an NLD (`{{...}}`). It acts like a method but is invoked using a more expressive, human-readable syntax (e.g., `DO {{perform some action with @param on @this}}`).
	
    - The existence of `@this` determine if the NLI is a method or a function.

7. **Logical, Not Syntactically Enforced, Sections**:
    
    - The division into "Attributes Section," "Methods Section," and "Operators Section" as shown above (often separated by comments) is a **strong convention for readability and organization**.
    
    - However, AlgoDraft does **not** syntactically enforce these specific section demarcations or their order with special keywords (beyond the overall `CLASS ... ENDCLASS` block). You could, for example, interleave attribute and method definitions if you chose, though this is generally discouraged for clarity. The primary requirement is that attributes are declared before they are used (e.g., by methods within the same class). Adhering to the grouped structure is highly recommended for maintainable pseudocode.

This general structure provides the framework. In the following subsections, we will delve into the specific syntax and rules for defining attributes, understanding visibility, crafting methods (including the constructor), and optionally overloading operators.





