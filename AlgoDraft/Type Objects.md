---
status: Writing
---
In AlgoDraft, understanding and working with data types goes beyond simply declaring variables. Types themselves are treated as **first-class entities**, meaning that information about a type can be obtained, stored in variables, compared, and used to control the logic of your algorithms. This capability provides a powerful way to design more flexible, robust, and self-aware algorithms.

At the heart of this system is the concept of a **Type Object**. Every distinct data type available in AlgoDraft, whether it's a built-in primitive like `Integer`, a user-defined `RECORD` or `CLASS`, or a conceptual type declared in a `CONCEPT` block, is uniquely represented by a Type Object.

# The `Type` Data Type

AlgoDraft includes a special, built-in data type called **`Type`**. Variables of this Type data type are used to hold **Type Objects**.

- **Definition:** A **Type Object** is an immutable, internal AlgoDraft representation that uniquely identifies and describes a specific data type (e.g., the type `Integer`, the type `String`, a user-defined type `MyEmployeeRecord`, or a conceptual type `ExternalAPIResponse`). It's not the data instance itself, but rather the blueprint or description of that data's kind.

- **Conceptual Nature:** You can think of a Type Object as encapsulating information about a type, most notably its name. While you don't typically interact with a Type Object's internal properties directly in AlgoDraft, its primary role is to be a distinct entity that can be compared and used in type-specific logic.

- **Equality:** Type Objects support equality comparison. Two Type Objects are considered equal if and only if they represent the exact same data type. This is because the Type data type conceptually adheres to the IEquatable interface.

**Syntax:**

```
CLASS Type IMPLEMENTS IEquatable
```

# Type Literals: Referring to Types Directly

In AlgoDraft, the standard identifiers used to name types (e.g., `Integer`, `Boolean`, `MyCustomClass`, `ImportedType`) also serve as **type literals**. When a type identifier is used in a context expecting a Type Object (like assignment to a variable of type Type, or in a `CASE` clause of a `MATCH TYPE OF` statement), it evaluates to its corresponding unique Type Object.

**Example:**

```
// Declaring variables of type Type and assigning Type Objects using type literals
$intType AS Type <- Integer
$stringType AS Type <- String

CONCEPT
    RECORD UserProfileData := {{Conceptual record for user profiles}}
ENDCONCEPT
$userProfileType AS Type <- UserProfileData // UserProfileData is a type literal

IF $intType = Integer THEN // Comparing a Type Object with a type literal
    NOTIFY {{the variable $intType indeed holds the Type Object for Integer}}
ENDIF

$anotherType AS Type
$anotherType <- $stringType // Assigning one Type Object to another variable of type Type
IF $anotherType = String THEN
    NOTIFY {{both $anotherType and $stringType now refer to the String Type Object}}
ENDIF
```

This allows you to work with type information programmatically within your algorithms.
