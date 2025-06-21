---
status: Draft
---
In the design and implementation of algorithms, the ability to determine if two objects or values are "equal" is a fundamental and frequently performed operation. This concept of equality is essential for a wide range of tasks, including:

- Searching for specific items within collections of data.

- Ensuring uniqueness of elements in data structures like Sets.

- Looking up values in Maps using keys.

- Implementing conditional logic that behaves differently based on whether two items represent the same conceptual value.

- Many comparison-based algorithms, such as those used in sorting or data validation.

When discussing equality, it's useful to distinguish between two common interpretations:

- **Identity (Reference Equality):** This checks if two variables or expressions refer to the exact same object instance in memory. While this is relevant in many implemented programming languages, AlgoDraft, as a conceptual design language, focuses less on these low-level memory details.

- **Value Equality (Equivalence):** This checks if two objects represent the same conceptual value, even if they are distinct instances. For example, two different Temperature objects, both representing 0 degrees Celsius, would be considered equal in value. Similarly, the integer 10 and the real number 10.0 are considered equivalent in many contexts. This notion of value equality is the primary focus of AlgoDraft's `IEquatable` interface.

The `IEquatable` interface in AlgoDraft provides the standard contract that allows user-defined classes (the primary way to define structured types in AlgoDraft) to specify their own custom logic for determining value equality.

# Special Values

AlgoDraft has specific system-level rules for equality comparisons involving special values. These rules are applied **before** any class's `IEquatable` implementation is invoked if one or both operands are special values:

## `NULL`

- `NULL = NULL` always evaluates to `TRUE`.

- `NULL = <non_null_value>` (where `<non_null_value>` is any actual object instance or any non-null basic type value) always evaluates to `FALSE`.

This means that when an `OPERATOR this = (other AS ANY)` is called, if one of the operands are `NULL`, the implementation will not be called but rather the language evaluates it according to rules.

# Syntax

```
INTERFACE IEquatable :=
    // Checks if the current object ('this') is considered equal to the 'other' object.
    OPERATOR this = (other AS ANY) -> Boolean ENDOPERATOR

    // Checks if the current object ('this') is considered not equal to the 'other' object.
    // (Uses the mathematical not equal symbol ≠)
    OPERATOR this ≠ (other AS ANY) -> Boolean ENDOPERATOR

    // Checks if the current object ('this') is considered not equal to the 'other' object.
    // (Uses the common programming not equal symbol !=)
    OPERATOR this != (other AS ANY) -> Boolean ENDOPERATOR
ENDINTERFACE
```

**Operator Contracts:**

- `OPERATOR this = (other AS ANY) -> Boolean`:
    
    - This is the core operator. A class implementing `IEquatable` must provide the logic to determine if the current instance (`this`) is considered equal to the other object (which can be of `ANY` type).
    
    - A well-behaved equality operator should ideally exhibit the following mathematical properties:
        
        - **Reflexivity:** For any non-null object `x`, `x = x` should ideally be `TRUE` (if `x`'s type implements `IEquatable` correctly).
        
        - **Symmetry:** If `x = y` evaluates to `TRUE`, then `y = x` should also evaluate to `TRUE`.
        
        - **Transitivity:** If `x = y` is `TRUE` and `y = z` is `TRUE`, then `x = z` should also be `TRUE`.

- `OPERATOR this ≠ (other AS ANY) -> Boolean` and `OPERATOR this != (other AS ANY) -> Boolean`:
    
    - These operators represent inequality. They are functionally identical.
    
    - **AlgoDraft Rule:** If a class implements the `=` operator from `IEquatable`, the `≠` and `!=` operators are **implicitly provided** by AlgoDraft and their behavior is defined as `NOT (this = other)`. A class can explicitly override these derived implementations if highly specialized behavior is needed, though this is rare.

# Semantics

When an equality expression `var1 = var2` is encountered, AlgoDraft's system follows a defined dispatch logic to determine the result:

```
IF {{@var1 is a special value}} OR {{@var2 is a special value}} THEN
    RETURN DO {{determine the result according to the rules}}
ENDIF
IF var1 IS AN IEquatable THEN
    TRY
        RETURN var1 = var2
    CATCH {{equality implementation error}}
        DO NOTHING
    ENDTRY
ENDIF
IF var2 IS AN IEquatable THEN
    RETURN var2 = var1
ELSE
    RAISE {{equality implementation error}}
ENDIF
```

The verbose form that contains the evaluation steps would be:

```
// Conceptual system-level evaluation logic for: result <- object1 = object2

// 1. Handling special alues first...
IF {{object1 is a special value}} OR {{object2 is a special value}} THEN
    RETURN DO {{determine the result according to the rules}}
ENDIF

// 2. Attempting object1's IEquatable implementation
IF object1 IS AN IEquatable THEN
    TRY
        // Invoke object1's implemented OPERATOR this = (other AS ANY) -> Boolean
        op1Result AS Boolean <- object1 = object2
        RETURN op1Result // Returns TRUE or FALSE if object1's operator handled it successfully
    CATCH {{equality implementation error}} // Specific error from object1's '='
        // This means object1's '=' explicitly stated it cannot compare with object2's type.
        // We will now fall through to try object2's perspective (Step 3).
        DO NOTHING
    // Any other unhandled error from within object1's '=' logic (not specifically
    // {{equality implementation error}}) would propagate immediately and stop evaluation here.
    ENDTRY
ENDIF

// 3. Attempt object2's IEquatable implementation
// This step is reached if object1 is not IEquatable, or if object1's '=' operator 
// raised the specific {{equality implementation error}}.
IF object2 IS AN IEquatable THEN
    TRY
        // Invoke object2's implemented OPERATOR this = (other AS ANY) -> Boolean,
        // effectively performing a symmetric check: object2 = object1
        op2Result AS Boolean <- object2 = object1
        RETURN op2Result // Returns TRUE or FALSE if object2's operator handled it
    CATCH {{any error from object2's =}} // This includes {{equality implementation error}}
        // If object2's '=' also errors (even an {{equality implementation error}}),
        // it signifies that no defined pathway for comparison exists from either perspective.
        RAISE {{equality not defined or resolvable between types @(TYPE OF var1) and @(TYPE OF var2) after trying both perspectives}}
    ENDTRY
ENDIF

// 4. Final Fallback: No Defined Equality Pathway
// This is reached if neither object1 nor object2 is IEquatable, or if both are IEquatable
// but their attempts to compare resulted in {{equality implementation error}} being caught
// and no resolution was found.
RAISE {{equality implementation error: No defined equality pathway found between types @type1 and @type2}}
```

1. **Handling Special Values (e.g., `NULL`):**  
    Before attempting any complex type-specific comparisons, AlgoDraft first checks if either `object1` or `object2` is a special, universally understood value like `NULL`.
    
    - If both are `NULL`, they are considered **equal** (`TRUE`).
    
    - If one is `NULL` and the other is not, they are considered **not equal** (`FALSE`).  
        If this step resolves the equality, the process stops here.

2. **Attempting Comparison from `object1`'s Perspective:**  
    If neither object was a special values that resolved equality, AlgoDraft then checks if the type of `object1` formally declares how it handles equality by implementing the `IEquatable` interface.
    
    - If `object1`'s type does implement `IEquatable`, AlgoDraft invokes `object1`'s custom `=` operator, passing `object2` as the item to compare against.
        
        - **Success:** If this custom operator can meaningfully compare `object1` with `object2` and definitively determines they are either equal or not, it returns `TRUE` or `FALSE` accordingly. This becomes the final result of the `object1 = object2` expression.
        
        - **Cannot Compare:** If `object1`'s `=` operator determines that it cannot perform a meaningful comparison with the specific type of `object2` (e.g., trying to compare a `Temperature` object with a `FilePath` object using `Temperature`'s logic), it must signal this by raising a special `{{equality implementation error}}`. In this specific error case, AlgoDraft doesn't stop; it proceeds to the next step to see if `object2` can handle the comparison.
        
        - **Other Errors:** If any other unexpected error occurs within `object1`'s custom `=` operator logic, that error is immediately propagated, and the overall `object1 = object2` operation halts with that error.

3. **Attempting Comparison from `object2`'s Perspective (Symmetric Check):**  
    This step is reached only if `object1`'s type either doesn't implement `IEquatable` at all, or if its `=` operator specifically raised an `{{equality implementation error}}` (signaling it couldn't handle `object2`'s type). AlgoDraft then tries the comparison from `object2`'s viewpoint:
    
    - It checks if `object2`'s type implements `IEquatable`.
        
        - If `object2`'s type does implement `IEquatable`, AlgoDraft invokes `object2`'s custom `=` operator, this time passing `object1` as the item to compare against (effectively checking `object2 = object1`).
            
            - **Success:** If this returns `TRUE` or `FALSE`, that becomes the final result.
                
            - **Any Error (including inability to compare):** If `object2`'s `=` operator raises `{{equality implementation error}}`, that error is propagated as the final outcome. This means that if both sides signal they cannot perform the comparison,

4. **Final Fallback: Equality Undefined:**  
    If, after all the above steps, no defined pathway for comparison has been found (e.g., neither `object1` nor `object2` implements `IEquatable`, or both tried and signaled an `{{equality implementation error}}` specific to the other's type), AlgoDraft concludes that equality is not meaningfully defined for this pair of objects. In this case, the `object1 = object2` operation **raises an `{{equality implementation error}}`**, clearly indicating to the designer that a comparison contract is missing or incompatible.

**In essence:** AlgoDraft prioritizes explicit equality definitions. It first handles universal cases like `NULL`. Then, it gives `object1` a chance to define equality. If `object1` can't (either by not implementing `IEquatable` or by its implementation specifically rejecting `object2`'s type), it gives `object2` a chance. If neither can provide a definitive `TRUE` or `FALSE` through their `IEquatable` contract for the given pair, an error is raised, promoting clear and unambiguous algorithm design. 

# Implementation

A `CLASS` implementing `IEquatable` **MUST** provide a concrete implementation for **exactly one** of the three equality/inequality operators:

* `OPERATOR this = other -> Boolean` (Equality)  

* `OPERATOR this ≠ other -> Boolean` (Mathematical Inequality)  

* `OPERATOR this != other -> Boolean` (Common Programming Inequality)

The choice of which one to implement is up to the designer, though implementing the `=` operator is the most common and often the most natural starting point.

**Implicitly Provided Operators (Derivation by AlgoDraft):**

Once a class has provided the required implementation for one of these operators, AlgoDraft **implicitly provides** the logic for the remaining two equality/inequality operators. The derivation rules are straightforward:

- **If `=` is implemented by the class:**
    
    - `a != b` is implicitly defined as `NOT (a = b)`
    
    - `a ≠ b` is implicitly defined as `NOT (a = b)`

- **If `!=` is implemented by the class:**
    
    - `a = b` is implicitly defined as `NOT (a != b)`
    
    - `a ≠ b` is implicitly defined as `a != b`

- **If `≠` is implemented by the class:**
    
    - `a = b` is implicitly defined as `NOT (a ≠ b)`
    
    - `a != b` is implicitly defined as `a ≠ b`

A class can choose to explicitly override any of these implicitly provided operators if a highly specialized or perhaps more performant (though unlikely for simple negation) implementation is desired, but this is generally not necessary. This minimal requirement simplifies the task for the class designer while ensuring a complete and consistent set of equality operations.

# Example

User-defined `CLASS`es in AlgoDraft can choose to `IMPLEMENTS IEquatable` to provide custom equality logic tailored to their specific attributes and meaning:

```
CLASS Temperature IMPLEMENTS IEquatable :=
    value AS Real, // Temperature value, assuming a canonical unit like Celsius for simplicity

    METHOD NEW(IN val AS Real)
        this.value <- val
    ENDMETHOD

    // Implementing only the '=' operator to satisfy IEquatable
    OPERATOR this = (otherObj AS ANY) -> Boolean :=
        IF otherObj IS A Temperature THEN
            RETURN this.value = otherTemp.value // Uses Real type's built-in equality
        ELSE
            // This Temperature class's '=' operator only knows how to compare
            // with other Temperature instances. For any other type, it signals
            // an inability to perform the comparison from its perspective.
            RAISE {{equality implementation error: Temperature instance cannot compare itself with type @(TYPE OF otherObj)}}
        ENDIF
    ENDOPERATOR

    // Operators '!=' and '≠' are implicitly derived by AlgoDraft for the Temperature class
    // based on its implemented '=' operator. For example:
    // tempA != tempB   will evaluate as   NOT (tempA = tempB)
ENDCLASS
```

The equality operators are used in various contexts, most commonly in conditional statements:

```
tempC_ZeroA AS Temperature <- NEW Temperature(0.0)
tempC_ZeroB AS Temperature <- NEW Temperature(0.0)
tempC_Hundred AS Temperature <- NEW Temperature(100.0)

isZeroSame AS Boolean <- (tempC_ZeroA = tempC_ZeroB) // TRUE
NOTIFY {{Are tempC_ZeroA and tempC_ZeroB equal? @isZeroSame}}

isZeroHundredDifferent AS Boolean <- (tempC_ZeroA != tempC_Hundred) // TRUE (using derived !=)
NOTIFY {{Are tempC_ZeroA and tempC_Hundred different? @isZeroHundredDifferent}}

// Example of a comparison that would raise an error
CLASS Gizmo :=
	id AS Integer,
	METHOD NEW(val AS Integer) := this.id <- val
ENDCLASS // No IEquatable

gizmo1 AS Gizmo <- NEW Gizmo(1)
// comparisonResult AS Boolean <- (tempC_ZeroA = gizmo1) // This line would RAISE AN ERROR
// because Temperature's '=' would raise {{equality implementation error}} for Gizmo,
// and Gizmo does not implement IEquatable to attempt a symmetric comparison.
```

# Summary

- The `IEquatable` interface provides the standard contract for classes in AlgoDraft to define their own notion of **value equality** through the `=`, `≠`, and `!=` operators.

- Implementing classes need only define one of these three; AlgoDraft implicitly derives the others.

- Almost all AlgoDraft basic data types provide built-in `IEquatable` behavior for value comparisons.

- AlgoDraft's system for evaluating `object1 = object2` includes handling for universally special values and a dispatch mechanism that attempts comparison from both objects' perspectives if they implement `IEquatable`.

- Crucially, an `OPERATOR =` implementation within a class **must `RAISE {{equality implementation error ...}}`** if it cannot meaningfully compare this with the other object's type. This allows the system to try other pathways or ultimately determine that equality is undefined for that pair, rather than defaulting to a potentially incorrect `FALSE`.

- Proper implementation of `IEquatable` is essential for classes whose instances will be compared, stored in sets or used as map keys, or searched for in collections.

**Considerations for Implementing `=`:**

- **Type Checking:** Always check if the other object is of a type that your class can meaningfully compare against (as shown with `IF otherObj IS A Temperature THEN ...`).

- **Field Comparison:** Compare the relevant fields that define the "sameness" of your class instances. For some classes, all fields might matter; for others (like `Book` with ISBN), a unique identifier might be sufficient.

**Basic Types in AlgoDraft:**

* Almost all AlgoDraft basic data types **implement `IEquatable`**. Their `=` operator performs standard **value equality**:

- Two numbers are equal if they represent the same numerical value.

- Two strings are equal if they consist of the same sequence of characters.

- Two booleans are equal if they are both `TRUE` or both `FALSE`.

```
fiveA AS Integer <- 5
fiveB AS Integer <- 5
ten AS Integer <- 10
IF fiveA = fiveB THEN NOTIFY {{values @fiveA and @fiveB are equal}} ENDIF // TRUE

helloA AS String <- "hello"
helloB AS String <- "hello"
world AS String <- "world"
IF helloA = helloB THEN NOTIFY {{strings @helloA and @helloB are equal}} ENDIF // TRUE
IF helloA != world THEN NOTIFY {{strings @helloA and @world are not equal}} ENDIF // TRUE
```

**Mathematical Properties of Equality in AlgoDraft:**

- **Reflexivity (`x = x`):** An expression `x = x` will evaluate to `TRUE` only if `x`'s type implements `IEquatable` with an `=` operator that correctly returns `TRUE` when comparing an instance to itself. If `x`'s type does not implement `IEquatable`, `x = x` will raise `{{equality mplementation error}}`.

- **Symmetry (`x = y` implies `y = x`):** AlgoDraft's dispatch mechanism (by potentially trying both `x = y` and `y = x` if the first signals inability) attempts to support symmetry. However, the ultimate Boolean outcome still depends on the logic within the implemented `=` operators of the respective classes. Well-behaved `IEquatable` implementations should be designed to be symmetric.

**Use Cases:**

The IEquatable interface and its proper implementation are fundamental to the correct and predictable functioning of many other AlgoDraft features and common algorithmic patterns:

- **The `IN` operator of `IContainer`:** Relies on the `=` operator to check for membership.

- **Set data structures:** Use equality to ensure that all elements are unique.

- **Map data structures:** Use equality for comparing and looking up keys.

- **Searching algorithms:** Often need to check if a target element is equal to elements in a collection.

- **Removing duplicates from a list.**