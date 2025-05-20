---
status: Writing
---
The `TYPE OF` unary operator returns the type object of a declared or inferred type of a given object (typically represented by a variable).

**Syntax:**

```
TYPE OF object_operand
```

* **`object_operand`**: An identifier representing an object instance (e.g., a variable name). It is **not** a general expression that first needs evaluation; it directly inspects the type of the object the identifier refers to.

- The `TYPE OF` operator returns a **type object** such as `Integer`, `String`, `Boolean`, `MyRecord` (a user-defined record), `MyClass` (a user-defined class), `File<String>` (a generic type instance), or a conceptual type name like `UserSession` (declared in a `CONCEPT` block).

- Type objects can be compared for equality using the `=` operator (e.g., `(TYPE OF $obj1) = (TYPE OF $obj2)`).

- When used in contexts like `NOTIFY`, the type descriptor is presented in a human-readable form (typically its name).

**Semantics:**

- Examines the `object_operand`.

- Determines the most specific known type of the object instance currently held by `object_operand`.

- Returns the corresponding type descriptor.
	
	- For built-in types, it returns their standard AlgoDraft type objects (e.g., `Integer`, `String`).
	
	- For user-defined `RECORD`s and `CLASS`es, it returns the type object corresponding to their definition name.
	
	- For types declared in `CONCEPT` blocks or imported via `IMPORT`, it returns the AlgoDraft type object assigned to that conceptual or imported type.

**Use Cases:**

1. **Clarity in Design and Logging:** Explicitly stating or conveying the conceptual type of an object for documentation, debugging descriptions, or user notifications.

2. **Type-Based Dispatch:** Implementing logic that branches based on the exact type of an object, especially when a variable can hold instances of different, known types (e.g., using a `MATCH` construct).

3. **Comparing the Types of Objects:** Determining if two different objects are instances of the exact same type.

4. **Conceptual Metaprogramming:** In advanced designs, reasoning about types themselves (though less common in typical pseudocode).

# Examples

**Example 1: Basic Usage with Built-in Types:**

```
$typeA AS Type <- TYPE OF "some string" // Type Object for String
$typeB AS Type <- String             // Type Object for String (using type literal)
$typeC AS Type <- TYPE OF 100        // Type Object for Integer

IF $typeA = $typeB THEN
    NOTIFY {{typeA and typeB represent the same type (String)}}
ENDIF

IF $typeA != $typeC THEN
    NOTIFY {{typeA (String) and typeC (Integer) represent different types}}
ENDIF

$var1 AS ANY <- 5
$var2 AS ANY <- 10
IF (TYPE OF $var1) = (TYPE OF $var2) THEN
    NOTIFY {{$var1 and $var2 are instances of the same type}}
ENDIF
```

**Example 2: With User-Defined Types (`RECORD`, `CLASS`)**

```
RECORD Point := $x AS Integer, $y AS Integer ENDRECORD
CLASS Shape := $color AS String ENDCLASS
CLASS Circle INHERITS Shape := $radius AS Integer ENDCLASS

$p AS Point <- NEW Point(1, 1,)
$c AS Circle <- NEW Circle("black")
$s AS Shape <- $c // $s is declared as Shape, but holds a Circle instance

NOTIFY {{type of object $p is (TYPE OF $p)}}     // The user is informed that $p's object type is Point
NOTIFY {{type of object $c is (TYPE OF $c)}}     // The user is informed that $c's object type is Circle
NOTIFY {{type of object $s is (TYPE OF $s)}}     // The user is informed that $s's object type is Circle
                                              // (TYPE OF gives the most specific type of the instance)
```

**Example 3: With Conceptual (`CONCEPT`) or Imported (`IMPORT`) Types:**

```
CONCEPT
    TYPE UserSessionToken := {{An opaque token representing a user's session}}
ENDCONCEPT
IMPORT
    TYPE ExternalWidget FROM GuiFramework
ENDIMPORT

$token AS UserSessionToken <- GenerateToken() // Assume GenerateToken() returns a UserSessionToken
$uiWidget AS ExternalWidget <- CreateExternalWidget() // Assume this returns an ExternalWidget

NOTIFY {{type of $token is (TYPE OF $token)}}     // The user is informed that $token's type is UserSessionToken
NOTIFY {{type of $uiWidget is (TYPE OF $uiWidget)}} // The user is informed that $uiWidget's type is ExternalWidget
```

**Example 4: Type-Based Dispatch with MATCH TYPE OF:** A powerful use case for `TYPE OF` is in conditional logic based on an object's specific type, especially when a variable can hold instances of different types.

```
$documentPath AS String OR FsPath // Variable can hold either type

$documentPath <- DO {{read path from application settings, could be a String or an FsPath object}}

MATCH (TYPE OF $documentPath) AGAINST
    CASE String THEN
        NOTIFY {{document path is a string: $documentPath. processing as a raw string path.}}
        // ... logic specific to handling a string path ...
    CASE FsPath THEN
        NOTIFY {{document path is an FsPath object. processing using FsPath capabilities.}}
        // ... logic specific to handling an FsPath object ...
    ELSE // Optional: handles types not explicitly cased, or if $documentPath is uninitialized
        NOTIFY {{document path type is unexpected or unhandled: (TYPE OF $documentPath)}}
ENDMATCH
```

```
// Example with numeric types
$numericValue AS ANY <- INPUT {{a numeric data}}

MATCH TYPE OF $numericValue AGAINST
    CASE Integer THEN
        NOTIFY {{$numericValue is an integer}}
    CASE Float THEN
        NOTIFY {{$numericValue is a float}}
    CASE Complex THEN
        NOTIFY {{$numericValue is a complex number}}
    ELSE
        NOTIFY {{$numericValue is not a recognized numeric type for this operation}}
ENDMATCH
```

**Example 5: Comparing Types of Two Objects:** You can compare the type descriptors returned by `TYPE OF` to see if two objects are instances of the exact same type.

```
$item1 AS ANY <- 500
$item2 AS ANY <- 1000
$item3 AS ANY <- "500"

IF (TYPE OF $item1) = (TYPE OF $item2) THEN
    NOTIFY {{item1 and item2 are of the same type: (TYPE OF $item1)}}
ELSE
    NOTIFY {{item1 and item2 are of different types}}
ENDIF
// The user is informed that item1 and item2 are of the same type: Integer

IF (TYPE OF $item1) = (TYPE OF $item3) THEN
    NOTIFY {{item1 and item3 are of the same type}}
ELSE
    NOTIFY {{item1 (type: (TYPE OF $item1))) and item3 (type: (TYPE OF $item3))) are of different types}}
ENDIF
// The user is informed that item1 (type: Integer) and item3 (type: String) are of different types
```
