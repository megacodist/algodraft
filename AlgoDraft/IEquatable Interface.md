---
status: Draft
---
In programming and algorithm design, determining if two objects or values are "equal" is a fundamental and frequently performed operation. It's essential for:

- Searching for items within collections.

- Ensuring uniqueness in data structures like Sets.

- Looking up values in Maps using keys.

- Implementing conditional logic based on whether two items represent the same value.

- Many comparison-based algorithms.

When we talk about equality, it's useful to distinguish between:

- **Identity (Reference Equality):** This checks if two variables refer to the exact same object instance in memory. While relevant in many programming languages, AlgoDraft, as a conceptual design language, focuses less on this low-level implementation detail.

- **Value Equality (Equivalence):** This checks if two objects represent the same conceptual value, even if they are different instances. For example, two different Point objects, both representing the coordinates (10, 20), would be considered equal in value. This is the primary focus of the `IEquatable` interface.

The `IEquatable` interface in AlgoDraft provides the standard contract that allows user-defined `CLASS`es to define their own custom logic for value equality.

**Syntax:**

```
INTERFACE IEquatable :=
    // Checks if the current object ('this') is equal to the 'other' object.
    OPERATOR this = (other AS ANY) -> Boolean ENDOPERATOR
	
    // Checks if the current object ('this') is not equal to the 'other' object.
    // (Mathematical not equal symbol)
    OPERATOR this ≠ (other AS ANY) -> Boolean ENDOPERATOR
	
    // Checks if the current object ('this') is not equal to the 'other' object.
    // (Common programming not equal symbol)
    OPERATOR this != (other AS ANY) -> Boolean ENDOPERATOR
ENDINTERFACE
```

- `OPERATOR this = other -> Boolean`:
    
    - This is the core operator that an implementing class must define. It provides the logic to determine if the current instance (`this`) is considered equal to the other object instance.
    
    - A well-behaved equality operator should typically be:
        
        - **Reflexive:** `x = x` is always `TRUE`.
        
        - **Symmetric:** If `x = y` is `TRUE`, then `y = x` must also be `TRUE`.
        
        - **Transitive:** If `x = y` is `TRUE` and `y = z` is `TRUE`, then `x = z` must also be `TRUE`.
    
- `OPERATOR this ≠ other -> Boolean` and `OPERATOR this != other -> Boolean`:
    
    - These operators represent inequality.
        
    - Typically, their behavior is the logical negation of the `=` operator (i.e., they return `NOT (this = other)`).
    
    - **AlgoDraft Rule**: If a class implements the `=` operator from `IEquatable`, the `≠` and `!=` operators are implicitly available as its negation, unless the class explicitly provides its own overriding implementations for them (which is rare but possible).

**Semantics:**

The behavior of the equality operators in AlgoDraft is crucial for predictable algorithm design. When an equality operation like `object1 = object2` is evaluated:

1. The AlgoDraft system determines if a meaningful, defined comparison is possible between object1 and object2. This requires that **at least one of the classes of object1 or object2 implements `IEquatable` AND its defined `=` operator is capable of performing a comparison with an instance of the other object's class.**
    
    - **Dispatch Logic (Conceptual):**
        
        - If `object1`'s class implements `IEquatable` and its `=` operator is defined to handle the type of `object2`, that operator is invoked (e.g., `object1`'s definition of `= (object2)`).
        
        - If the first check fails (e.g., `object1`'s `=` operator cannot handle `object2`'s type), but `object2`'s class implements `IEquatable` and its `=` operator is defined to handle the type of `object1`, that operator might then be invoked (e.g., `object2`'s definition of `= (object1)`), upholding symmetry. For simplicity, it's often best if an `=` operator designed for inter-type comparison takes `ANY` as its `other` parameter and performs internal type checks.
    
2. **Outcome of a Defined Comparison:**
    
    - If a capable `IEquatable` implementation is found and executed, the operation returns `TRUE` if the objects are considered equal by that specific implementation, or `FALSE` if they are considered unequal.
    
3. **Error Condition (Undefined Comparison):**
    
    - The equality operation (`=`, `≠`, or `!=`) will **RAISE AN ERROR** if:
        
        - Neither `object1`'s class nor `object2`'s class implements `IEquatable`.
        
        - Both (or one) implement `IEquatable`, but neither of their respective `=` operators is defined in a way that can meaningfully compare with an instance of the other object's class (e.g., `ClassA`'s = only accepts `ClassA`, `ClassB`'s = only accepts `ClassB`, and you try to compare an instance of `ClassA` with `ClassB`).
        
    - **Why an Error?** Raising an error for undefined equality comparisons is critical. It signals a potential design flaw or a missing contract, forcing the designer to explicitly address how such disparate objects should (or should not) be compared, rather than the system silently defaulting to a potentially misleading `FALSE` or an arbitrary comparison.

**Example:**

User-defined `CLASS`es in AlgoDraft can choose to `IMPLEMENTS IEquatable` to provide custom equality logic tailored to their specific attributes and meaning:

```
CLASS Book IMPLEMENTS IEquatable :=
    title AS String
    author AS String
    isbn AS String // International Standard Book Number, usually unique

    METHOD NEW(
		    IN title AS String,
		    IN author AS String,
		    IN isbn AS String,
		    ) :=
        this.title <- title
        this.author <- author
        this.isbn <- isbn
    ENDMETHOD
	
    // Implementing the '=' operator from IEquatable
    // For books, equality is often best determined by their unique ISBN.
    OPERATOR this = otherObj AS ANY -> Boolean :=
        IF otherObj IS A Book THEN
            RETURN this.isbn = otherBook.isbn // Compare based on ISBN
        ELSE
            // A Book can only be meaningfully compared for equality with another Book.
            // If 'otherObj' is not a Book, this specific operator considers them not equal.
            // The overall '=' evaluation will then depend on whether otherObj's type
            // can compare itself to a Book.
            RETURN FALSE
        ENDIF
    ENDOPERATOR
	
    // != and ≠ are implicitly available as NOT (this = otherObj)
    OPERATOR this != otherObj AS ANY -> Boolean := RETURN NOT (this = otherObj) ENDOPERATOR
    OPERATOR this ≠ otherObj AS ANY -> Boolean := RETURN NOT (this = otherObj) ENDOPERATOR
ENDCLASS
```

The equality operators are used in various contexts, most commonly in conditional statements:

```
book1 AS Book <- NEW Book("AlgoDraft Guide", "Google Gemini", "123-ABC")
book2 AS Book <- NEW Book("Advanced Algorithms", "Megacodist", "456-DEF")
book3 AS Book <- NEW Book("AlgoDraft Guide", "Another Author", "123-ABC") // Same ISBN as book1

areProcessed AS Boolean <- FALSE

IF book1 = book3 THEN // TRUE, because Book's '=' compares ISBNs
    NOTIFY {{@book1 and @book3 are considered the same book @(same ISBN)}}
    areProcessed <- TRUE
ELSE
    NOTIFY {{@book1 and @book3 are different books}}
ENDIF

IF book1 != book2 THEN // TRUE
    NOTIFY {{@book1 and @book2 are different books}}
ENDIF

// Example that would cause an error:
CLASS Widget :=
	id AS Integer

	METHOD NEW(id AS Integer) :=
		this.id <- id
	ENDMETHOD
ENDCLASS // Does NOT implement IEquatable

widgetA AS Widget <- NEW Widget(20)
widgetB AS Widget <- NEW Widget(30)
// IF widgetA = widgetB THEN ... ENDIF // This line RAISES AN ERROR
```

**Considerations for Implementing `=`:**

- **Type Checking:** Always check if the other object is of a type that your class can meaningfully compare against (as shown with `IF otherObj IS A Book THEN ...`).

- **Field Comparison:** Compare the relevant fields that define the "sameness" of your class instances. For some classes, all fields might matter; for others (like `Book` with ISBN), a unique identifier might be sufficient.

- **Null/Uninitialized Values:** Consider how uninitialized object references or fields should be handled in equality checks if AlgoDraft supports such states. (For simplicity, AlgoDraft examples often assume valid, initialized objects).

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

**Use Cases:**

The IEquatable interface and its proper implementation are fundamental to the correct and predictable functioning of many other AlgoDraft features and common algorithmic patterns:

- **The `IN` operator of `IContainer`:** Relies on the `=` operator to check for membership.

- **Set data structures:** Use equality to ensure that all elements are unique.

- **Map data structures:** Use equality for comparing and looking up keys.

- **Searching algorithms:** Often need to check if a target element is equal to elements in a collection.

- **Removing duplicates from a list.**