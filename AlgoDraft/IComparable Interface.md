---
status: Draft
---
Beyond simply knowing if two objects are equal, many algorithms require the ability to determine their relative order – is one object "less than," "greater than," or "equal to" another? This concept of ordering is fundamental for:

- **Sorting:** Arranging elements in collections (like `List`s) into ascending or descending sequences.

- **Searching in Sorted Collections:** Enabling efficient search algorithms, such as binary search.

- **Ordered Data Structures:** Implementing data structures like priority queues, ordered sets, or sorted maps, which inherently maintain their elements in a specific order.

- **Range Queries:** Finding elements that fall within a certain value range.

- **Logical Conditions:** Using operators like `<`, `>`, `<=`, `>=` in conditional statements.

The `IComparable` interface in AlgoDraft provides the standard contract that allows user-defined `CLASS`es to specify their own custom logic for how their instances should be ordered relative to one another. Since the ability to order objects implies the ability to check for equality, the `IComparable` interface in AlgoDraft `INHERITS` from (and thus builds upon) the `IEquatable` interface.

# Syntax

The `IComparable` interface is a built-in interface in AlgoDraft, formally defined as:

```
// Prerequisite interface:
// INTERFACE IEquatable :=
//     OPERATOR this = other -> Boolean ENDOPERATOR
//     OPERATOR this ≠ other -> Boolean ENDOPERATOR
//     OPERATOR this != other -> Boolean ENDOPERATOR
// ENDINTERFACE

INTERFACE IComparable INHERITS IEquatable :=
    // Ordering operators
    OPERATOR this < other -> Boolean ENDOPERATOR  // Less than
    OPERATOR this > other -> Boolean ENDOPERATOR  // Greater than
    OPERATOR this <= other -> Boolean ENDOPERATOR // Less than or equal to
    OPERATOR this ≤ other -> Boolean ENDOPERATOR  // Less than or equal to (mathematical symbol)
    OPERATOR this >= other -> Boolean ENDOPERATOR // Greater than or equal to
    OPERATOR this ≥ other -> Boolean ENDOPERATOR  // Greater than or equal to (mathematical symbol)

    // Equality operators (this = other, this ≠ other, this != other)
    // are inherited from the IEquatable interface.
ENDINTERFACE
```

This interface defines a comprehensive set of six ordering operators. However, as we'll see next, a class implementing `IComparable` only needs to provide logic for a minimal subset of these.

# Semantics

When a comparison operator like `object1 < object2` is evaluated in AlgoDraft:

1. The system determines if a meaningful, defined ordering comparison is possible. This requires that **at least one of the classes of object1 or object2 implements `IComparable`** (which includes providing the necessary basis for equality and one ordering operator) **AND** that the effective operator (either directly implemented or derived by AlgoDraft) can handle the types of both operands.

2. If such a comparison is defined and can be executed:
    
    - The operation returns `TRUE` or `FALSE` according to the defined ordering logic.

3. If **no such defined ordering pathway exists** between the specific types of `object1` and `object2` (e.g., neither class implements `IComparable` adequately, or they implement it but in ways that don't allow for mutual comparison):
    
    - The comparison operation **raises an error**. This signifies an undefined or invalid attempt to order the objects.

This strict error handling ensures that ordering is only performed when it is explicitly and meaningfully defined by the types involved.

# Implementation

User-defined `CLASS`es in AlgoDraft can `IMPLEMENTS IComparable` to enable meaningful, custom ordering for their instances. To successfully implement `IComparable` (and by extension, `IEquatable`), a `CLASS` **MUST** provide concrete implementations for:

1. **Exactly one** of the equality/inequality operators inherited from `IEquatable`:
    
    - `OPERATOR this = other -> Boolean` (Most commonly implemented)
    
    - **OR** `OPERATOR this ≠ other -> Boolean`
    
    - **OR** `OPERATOR this != other -> Boolean`

2. **Exactly one** of the six ordering operators defined directly in `IComparable`:
    
    - `OPERATOR this < other -> Boolean`
    
    - **OR** `OPERATOR this > other -> Boolean`
    
    - **OR** `OPERATOR this <= other -> Boolean`
    
    - **OR** `OPERATOR this ≤ other -> Boolean`
    
    - **OR** `OPERATOR this >= other -> Boolean`
    
    - **OR** `OPERATOR this ≥ other -> Boolean`

The choice of which specific equality operator and which specific ordering operator to implement is entirely up to the designer, based on what is most logical or straightforward for the class's comparison semantics.

## Implicitly Provided Operators (Derivation by AlgoDraft)

Once a class has provided the required minimal implementations (one for equality, one for ordering), AlgoDraft **implicitly provides** the logic for all other five ordering operators and the remaining two equality/inequality operators.

- **Example Derivation:** If a class implements = and <:
    
    - a != b and a ≠ b are derived as NOT (a = b).
        
    - a > b is derived as NOT (a < b OR a = b).
        
    - a <= b (and a ≤ b) is derived as (a < b) OR (a = b).
        
    - a >= b (and a ≥ b) is derived as NOT (a < b).  
        (The AlgoDraft language specification will detail the precise derivation rules for all combinations of minimally implemented operators.)
        

A class can choose to explicitly override any of these implicitly provided operators if a more specialized or efficient implementation is necessary, but this is generally not required.

# Examples

**Example 1:**

This example defines `Temperature` class implementing `=` and `<`.

```
// CLASS Temperature implements both IEquatable (for '=') and IComparable (for '<')
CLASS Temperature IMPLEMENTS IComparable :=
    valueCelsius AS Real // Temperature value in Celsius

    METHOD NEW(IN degreesCelsius AS Real)
        this.valueCelsius <- degreesCelsius
    ENDMETHOD

    // --- Implemented to satisfy IEquatable ---
    OPERATOR this = otherObj AS ANY -> Boolean :=
        IF otherObj IS A Temperature THEN
            otherTemp AS Temperature <- otherObj AS Temperature // Conceptual cast
            RETURN this.valueCelsius = otherTemp.valueCelsius
        ELSE
            // According to IEquatable semantics, if comparison isn't defined, an error is raised.
            RAISE {{failed to check equality between @Temperature and @(TYPE OF otherObj)}}
        ENDIF
    ENDOPERATOR
    // Operators '!=' and '≠' are implicitly derived by AlgoDraft for Temperature.

    // --- Implemented to satisfy IComparable (choosing '<' as the single ordering operator) ---
    OPERATOR this < otherObj AS ANY -> Boolean :=
        IF otherObj IS A Temperature THEN
            otherTemp AS Temperature <- otherObj AS Temperature // Conceptual cast
            RETURN this.valueCelsius < otherTemp.valueCelsius
        ELSE
            // According to IComparable semantics, if comparison isn't defined, an error is raised.
            RAISE {{failed to check order between @Temperature and @(TYPE OF otherObj)}}
        ENDIF
    ENDOPERATOR

    // Operators >, <=, ≤, >=, and ≥ are implicitly derived by AlgoDraft
    // for the Temperature class, based on its implemented '=' and '<' operators.
ENDCLASS
```

- **Type Checking:** Within your implemented operator (e.g., the custom `<` or `=`), always check if the `otherObj` is of a type that your class can meaningfully compare against.

- **Error Handling:** If a comparison is attempted with an incompatible type for which no logic is defined, the operator should raise an error (as shown in the `Temperature` example). This adheres to the strict comparison semantics of AlgoDraft.

- **Consistency:** Ensure your implemented logic is consistent (e.g., if `a < b` is `TRUE`, and `a = b` is `FALSE`, then `a <= b` should also be `TRUE`). The derivation rules in AlgoDraft aim to maintain this consistency.

**Example 2:**

The primary use of these operators is within conditional logic and, most importantly, to enable functionality that relies on ordered data.

```
freezingPointC AS Temperature <- NEW Temperature(0.0)
currentTempC AS Temperature <- NEW Temperature(-5.2)
boilingPointC AS Temperature <- NEW Temperature(100.0)

IF currentTempC < freezingPointC THEN
    NOTIFY {{current temperature @currentTempC.valueCelsius °C is below freezing}}
ENDIF

IF currentTempC >= boilingPointC THEN
    NOTIFY {{water would be boiling at current temperature @currentTempC.valueCelsius °C}}
ELSE
    NOTIFY {{water would not be boiling at current temperature @currentTempC.valueCelsius °C}}
ENDIF

// Conceptual use in sorting algorithms:
temperatureReadings AS List<Temperature> <- [freezingPointC, currentTempC, boilingPointC, NEW Temperature(22.5)]
// The following operation relies on Temperature implementing IComparable:
// sortedTemps AS List<Temperature> <- DO {{Sort @temperatureReadings}}

// NOTIFY {{sorted temperatures are @sortedTemps}}
```

# Notes

**`IComparable` for Built-in Basic Data Types**:

Many of AlgoDraft's built-in basic data types implicitly implement `IComparable` and provide standard ordering logic:

- **`Integer`, `Float`:** Implement `IComparable` using standard numerical ordering. (e.g., `5 < 10` is `TRUE`).

- **`String`:** Implements `IComparable` using lexicographical (dictionary) ordering. (e.g., `"apple" < "banana"` is `TRUE`).

- **`Boolean`:** Implements `IComparable`. AlgoDraft defines  `FALSE < TRUE`.

- **`Char`**: Implements `IComparable` based on character codes (e.g., Unicode values).

- **`Date`, `Time`, `DateTime`:** Implement `IComparable` using chronological ordering.

**Basic Types NOT Implementing `IComparable` (Beyond Equality from `IEquatable`):**  
Certain basic types implement `IEquatable` (for equality checks) but do **not** implement `IComparable` because they lack a universally standard or natural total ordering:

- **`Complex`:** Mathematics says `Complex` numbers do not have a standard total ordering.

- **`FsPath`**

**Example:**

```
IF 25.5 > 10.0 THEN NOTIFY {{25.5 is numerically greater than 10.0}} ENDIF
IF "zebra" >= "aardvark" THEN NOTIFY {{"zebra" comes after or is the same as "aardvark" in dictionary order}} ENDIF
```

**The `IComparable` interface is vital for:**

- **Sorting Collections:** Generic sorting algorithms (for `List`s, etc.) depend on `IComparable` to order elements of user-defined types.

- **Ordered Data Structures:** Essential for classes that internally maintain their elements in a sorted order, such as ordered lists, priority queues, or the keys in sorted maps.

- **Efficient Searching:** Algorithms like binary search can only operate on collections that are sorted, which requires elements to be comparable.

- **Range Queries:** Finding elements that fall between a lower and upper bound.

- **Establishing Business Rules:** Many application-specific rules involve comparing values (e.g., "is this score greater than the passing threshold?").

 **Summary**:

- The `IComparable` interface, which inherits from `IEquatable`, provides the standard AlgoDraft contract for `CLASS`es to define their natural ordering logic.

- **Implementation is flexible**: a class only needs to define one operator for equality (typically `=`) and **any one** of the six ordering operators (e.g., `<`, `>`, `<=`, etc.). AlgoDraft then implicitly provides the remaining comparison operators.

- All relevant basic data types in AlgoDraft come with a predefined `IComparable` implementation.

- This interface is fundamental for algorithms involving sorting, ordered collections, and efficient searching.

- **AlgoDraft enforces defined ordering**: attempts to compare objects where no clear ordering path exists (via `IComparable` implementations) will result in an error.