---
status: Writing
---
Often in algorithm design, you need to treat several related pieces of data as a single unit. For example, a user profile might consist of a username, an ID, and an active status. Instead of managing separate variables for each piece of data everywhere, AlgoDraft allows you to define a **Record** to group these related data fields together under a custom type name.

A record is a data structure that aggregates multiple **fields**, where each field consists of a unique name (or label) and an associated value (name/label-value pairing).

A **Record** is a **heterogeneous, mutable, non-iterable, and non-duplicable associative** data structure in AlgoDraft:

- **Associative**: A record consists of **named/labeled values**, each with a **unique identifier** (name or label) and a corresponding value. The relationship is **name/label-value-pair mapping**, where:
    
    - Each name/label is **explicitly declared** in the record's structure.
    
    - Values are **accessed by name/label**, not by index or position.

- **Heterogeneous**: Each field in a record may hold a value of a **different type**. For example, a record may have three fields consisting of an `Integer`, a `String`, and a `Boolean`.

- **Mutable**: The values of a record’s fields can be **updated** after construction. However, **the field names and their existence are fixed** at definition time.

- **Non-iterable**: Records **do not implement** the `IIterable` interface by default. This means their fields cannot be traversed automatically using `FOR EACH` or similar syntax. Custom iteration (e.g., `FieldList OF`) must be explicitly defined if desired.

- **Non-duplicable**: Field names in a record must be **unique**. A record cannot contain multiple fields with the same name. Values can be duplicated.

# Defining a Record

You define a new record type using the `RECORD` and `ENDRECORD` keywords. Inside this block, you declare the fields (data members) that make up the record.

**Syntax**:

```
RECORD RecordName // Must follow PascalCase convention for types
	
    // --- Fields Without Default Values ---
    // These fields MUST be listed first.
    // Their values MUST be provided via arguments when using CREATE.
    $fieldNameA AS DataTypeA
    $fieldNameB AS DataTypeB
    // ... more fields without defaults
	
    // --- Fields With Default Values ---
    // These fields MUST be listed after fields without defaults.
    // They are assigned the specified default value during CREATE
    // unless explicitly overridden via arguments.
    // Default value assignment uses the AlgoDraft assignment operator '<-'.
    $fieldNameC AS DataTypeC <- <defaultValueC>
    $fieldNameD AS DataTypeD <- <defaultValueD>
    // ... more fields with defaults
	
ENDRECORD
```

**Keywords and Parts:**

- **`RECORD`**: The keyword that begins the definition of a new record type.

- **`RecordName`**: The name you choose for your custom record type (e.g., `UserProfile`, `Point`, `ColorRgb`). This name must follow the PascalCase convention for types in AlgoDraft.

- **`$fieldName`**: The name given to each piece of data (a field) within the record. Field names must be conformed with variable naming rule. Each field declaration must be on its own line and indented. Fields without default values (`<- value`) MUST be declared before fields with default values.

- **`AS DataType`**: The specified data type for the field which can be a basic data type, data structure, or a user-defined type.
- **`<- <defaultValue>`**: Optionally assigns a default value to a field using AlgoDraft's assignment operator (`<-`). The `<defaultValue>` must be a literal or existing object compatible with the `DataType`. Fields with defaults must come after fields without defaults.

- **`ENDRECORD`**: The keyword that marks the end of the record type definition block.

**Example 1:**

Let's define a record to hold information about a 2D point.

```
RECORD Point2D
    $xCoord AS Real
    $yCoord AS Real
ENDRECORD
```

This defines a new type called `Point2D` that groups two `Real` values, `$xCoord` and `$yCoord`.

**Example** 2:

Let's define a record for a user session, requiring `sessionId` and `userId` at creation, but providing defaults for others.

```
RECORD UserSession
    // Fields without default values (must be provided at creation)
    $sessionId AS String
    $userId AS Integer
	
    // Fields with default values (can be omitted or overridden at creation)
    $startTime AS DateTime <- DO {{get date and time during instantiation}}
    $isActive AS Boolean <- TRUE
    $permissions AS String <- "ReadOnly"
ENDRECORD
```

# Declaring Record Variables

Once a record type is defined, you can declare variables of that type using the variable declaration syntax:

**Example 1:**

```
$startPoint AS Point2D
$endPoint AS Point2D
```

This declares two variables, `$startPoint` and `$endPoint`, which are intended to hold instances of our `Point2D` record. At this point, they don't hold a specific point yet.

**Example 2:**

```
$currentSession AS UserSession
$adminSession AS UserSession
```

These variables are now ready to hold instances of the `UserSession` record.
# Creating Record Instances (Instantiation)

To create an actual data instance based on your `RECORD` definition, you use the **`NEW`** keyword followed by the record type name. Think of `NEW RecordType()` as invoking a special kind of **constructor function** that builds and returns a new instance of your record.

Because it behaves like a function call, the process of providing initial values (arguments) for the record's fields uses the **exact same argument-parameter-correspondence syntax and rules as regular function invocation** in AlgoDraft. For more information, refer to [[Function Invocation]]. 


**Example 1:**
```
$startPoint AS Point2D
$startPoint <- CREATE Point2D(0, 0) // Creates a Point2D instance

$endPoint AS Point2D
$endPoint <- CREATE Point2D(2, 2)   // Creates another Point2D instance
```

**Example 2:**
```
// Style 1: Using only Positional Arguments (for the required fields)
// startTime, isActive, permissions will get their defined defaults.
$session1 AS UserSession <- NEW UserSession("sess_abc", 123) // Positional for $sessionId, $userId

// Style 2: Using only Named Arguments (Possible if ALL fields had defaults, OR for overriding)
// Note: You MUST still provide values for $sessionId and $userId via named arguments here.
$session2 AS UserSession <- NEW UserSession(sessionId <- "sess_333", userId <- 555, permissions <- "Audit") // startTime and isActive get defaults

// Style 3: Using Mixed Arguments (Positional for required, Named for overriding defaults)
// startTime and isActive get defaults. Permissions is overridden.
$session3 AS UserSession <- NEW UserSession("sess_xyz", 456, permissions <- "ReadWrite")

// Style 4: Using Mixed Arguments (Overriding multiple defaults)
// startTime gets its default. isActive and permissions are overridden.
$session4 AS UserSession <- NEW UserSession("sess_111", 789, isActive <- FALSE, permissions <- "Admin")

// Style 5: Using Mixed Arguments (Overriding all defaults)
$specificTime AS DateTime // Assume this is set elsewhere
$session5 AS UserSession <- NEW UserSession("sess_222", 101, startTime <- $specificTime, isActive <- TRUE, permissions <- "Guest")

// --- INVALID Instantiation ---
// Missing arguments for required fields $sessionId, $userId
// $invalidSession AS UserSession <- NEW UserSession() // ERROR!
// $invalidSession AS UserSession <- NEW UserSession("sess_only") // ERROR! needs $userId too
// $invalidSession AS UserSession <- NEW UserSession(ermissions <- "Admin") // ERROR! needs $sessionId and $userId
```
