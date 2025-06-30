---
status: Draft
---
# The `IIntegerStream` Interface: The Basic Contract for Integer Streams

The `IIntegerStream` interface is a fundamental contract in AlgoDraft for any type whose primary role is to produce a **stream of integer values**. It ensures that all such integer streams, regardless of how they are internally specified (e.g., by boundaries, by count, or by more complex logic), can be treated uniformly in contexts that require iteration over integers or equality comparison based on the content of the generated stream.

**Syntax:** 

The `IIntegerStream` interface builds upon the basic capabilities of iteration and equatability:

```
// Assumed prerequisite interfaces:
// INTERFACE IIterable<T> := OPERATOR ITERATOR OF this -> Iterator<T> ENDINTERFACE
// INTERFACE IEquatable :=
//     OPERATOR this = other AS ANY -> Boolean ENDOPERATOR
//     OPERATOR this != other AS ANY -> Boolean ENDOPERATOR
//     OPERATOR this ≠ other AS ANY -> Boolean ENDOPERATOR
// ENDINTERFACE

INTERFACE IIntegerStream IMPLEMENTS IIterable<Integer>, IEquatable
    // This interface currently adds no new methods or operators beyond those inherited
    // from IIterable<Integer> and IEquatable. Its primary role is to:
    // 1. Gather types that specifically generate streams of Integers under the same umbrella.
    // 2. Define the semantic basis for equality between any two such integer streams.

    // --- Inherited Contracts ---
    // From IIterable<Integer>:
    //   OPERATOR ITERATOR OF this -> Iterator<Integer>
    //   (This ensures the object can produce an iterator that yields integers,
    //    allowing its use in FOR EACH loops and other iteration contexts.)

    // From IEquatable:
    //   OPERATOR this = other AS ANY -> Boolean
    //   OPERATOR this != other AS ANY -> Boolean
    //   OPERATOR this ≠ other AS ANY -> Boolean
ENDINTERFACE
```

**Semantics for equality:**

- Two `IIntegerStream` instances are considered **equal** if and only if they **generate the exact same finite or infinite stream of integers.**

- **Order matters significantly:** An integer stream yielding 1, 2, 3 is **NOT** equal to an integer stream yielding 3, 2, 1, even if they contain the same numbers. The sequence must be identical.

- While this is the conceptual definition of equality, specific subtypes of `IIntegerStream` (like `ISteppedIntStream`, which `Interval` and `Count` implement) may provide more efficient, parameter-based methods for determining this equality rather than always resorting to full stream iteration.

# The `ISteppedIntStream` Interface: The Contract for Arithmetic Progressions

Many useful integer streams follow a regular pattern known as an **arithmetic progression**. This means they have a constant difference (a "step") between any two consecutive elements. Examples include simple counting `1, 2, 3, 4`, counting by twos `0, 2, 4, 6`, or counting down `5, 4, 3`.

The `ISteppedIntStream` interface is a specialized interface in AlgoDraft that implements `IIntegerStream`. It is designed specifically for types that generate such step-wise integer streams. AlgoDraft's built-in `Interval` and `Count` types are primary examples of types that fulfill this contract.

This interface provides methods to access the defining parameters of the arithmetic progression: its first element, its last element (if finite), its constant step, and its total count of elements (if finite). This allows for more insightful inspection of the stream's definition and enables highly efficient equality comparisons between different `ISteppedIntStream` instances based on these parameters.

**Syntax**:

```
INTERFACE ISteppedIntStream IMPLEMENTS IIntegerStream :=

    CONST step AS Integer,

    METHOD GetFirst() -> Integer OR NULL ENDMETHOD
    METHOD GetLast() -> Integer OR NULL OR INFINITY OR -INFINITY ENDMETHOD
    METHOD GetCount() -> Integer OR INFINITY ENDMETHOD

    // --- Inherited Contracts ---
    // From IIntegerStream (and thus IIterable<Integer>):
    //   OPERATOR ITERATOR OF this -> Iterator<Integer> ENDOPERATOR

    // From IIntegerStream (and thus IEquatable):
    //   OPERATOR this != other AS ANY -> Boolean (Implicitly derived if '=' is provided)
    //   OPERATOR this ≠ other AS ANY -> Boolean (Implicitly derived if '=' is provided)

    // Specialized equality for ISteppedIntStream instances.
    // This provides a more specific and efficient implementation for the '=' operator
    // when comparing two ISteppedIntStream instances.
    OPERATOR this = (other AS ANY) -> Boolean :=
        IF other IS NOT AN ISteppedIntStream THEN
            // This ISteppedIntStream's '=' operator is specialized for parametrically
            // comparing with another ISteppedIntStream. If other is not one,
            // this operator signals its inability to perform this specific optimized
            // comparison. The AlgoDraft system's general '=' dispatch logic
            // would then try other's '=' operator if available.
            RAISE {{equality implementation error: ISteppedIntStream parametric equality is undefined for type @(TYPE OF other)}}
        ENDIF

        // If we reach here, other IS AN ISteppedIntStream
        other AS ISteppedIntStream <- other AS ISteppedIntStream // Conceptual cast
        thisFirst AS Integer OR NULL <- this.GetFirst()
        otherFirst AS Integer OR NULL <- other.GetFirst()

		IF thisFirst = otherFirst THEN
            IF thisFirst = NULL THEN
	            // They are empty streams, so, they are equal...
                RETURN TRUE
            ELSE IF this.step = other.step AND this.GetLast() = other.GetLast() THEN
                RETURN TRUE
            ELSE
                RETURN FALSE
            ENDIF
        ELSE
	        // Their first yielded elements are not equal.
	        // So, they are streams.
            RETURN FALSE
        ENDIF
    ENDOPERATOR
ENDINTERFACE
```

- **`IMPLEMENTS IIntegerStream`**:
	
    This signifies that `ISteppedIntStream` provides all the capabilities of a basic integer stream, including being `IIterable<Integer>` (being iterable to yield integer values) and `IEquatable` (its instances can be compared for equality).

- **`CONST step AS Integer`**:
	
    This is a read-only property representing the constant difference (the "step") between any two consecutive elements in the arithmetic progression generated by the stream.
    
    - This step value is crucial for defining the progression.
    
    - For streams intended to produce multiple distinct elements, step must not be zero. (Specific implementers like `Interval` will enforce this; `Count` allows step to be zero).

- **`METHOD GetFirst() -> Integer OR NULL`**:
    
    - This method returns the very first integer element of the progression.
    
    - It returns `NULL` if the progression is defined to be empty (e.g., resulting from a `Count` literal with `COUNT 0`, or an `Interval` literal like `[5 .. 5)`).

- **`METHOD GetLast() -> Integer OR NULL OR INFINITY OR -INFINITY`**:
    
    - This method returns the last integer element of a finite progression.
    
    - If the progression contains only a single element, `GetLast()` returns the same value as `GetFirst()`.
    
    - It returns `NULL` if the progression is empty.
    
    - It returns the special keyword `INFINITY` if the progression is infinite and increasing (i.e., `this.step > 0`).
    
    - It returns  `-INFINITY` (negative infinity) if the progression is infinite and decreasing (i.e., `step < 0`).

- **`METHOD GetCount() -> Integer OR INFINITY`**:
    
    - This method returns the total number of elements in a finite progression.
    
    - It returns the special keyword `INFINITY` if the progression is infinite.
    
    - It returns `0` if the progression is empty.

- **`OPERATOR this = (other AS ANY) -> Boolean`**:
    
    - This is a specialized and efficient implementation of the equality operator, designed specifically for comparing two `ISteppedIntStream` instances.
    
    - **Behavior:**
        
        1. It first checks if `other` is also an `ISteppedIntStream`. If not, it signals its inability to perform this specialized parametric comparison by raising an `{{equality implementation error: ISteppedIntStream parametric equality is undefined for type @(TYPE OF other)}}`. This allows AlgoDraft's main `=` dispatch mechanism to potentially try `other`'s own equality operator.
        
        2. If `other` is an `ISteppedIntStream`, the operator then compares their first elements:
		
			* If they are not equal, it means the two `ISteppedIntStream` are not equal.
            
            - If first elements are equal, the operator then checks if they are `NULL` or not:
                
	            - If both are `NULL`, it means they are empty streams and hence they are equal.
			    
	            - If first elements are equal but not `NULL`, the operator checks steps and last elements against their counterparts. It is the last barrier of equality
	
    - This parametric comparison is significantly more efficient than element-by-element stream comparison, especially for long or infinite stepped streams.
    
    - The `!=` and `≠` operators are implicitly derived by AlgoDraft based on this `=` operator.

**Implementations**:

The primary built-in AlgoDraft types that implements `ISteppedIntStream` are:

- **`Interval`**: Defines an arithmetic progression using start and end boundaries, and an explicit or derived step.

- **`Count`**: Defines an arithmetic progression using a starting or ending anchor, a specific count of items (which can be `INFINITY`), and an explicit or derived step.

These types use their own defining parameters (e.g., `Interval`'s boundaries, `Count`'s anchor and quantity) to implement the methods `GetFirst()`, `GetLast()`, and `GetCount()`, as well as the specialized `=` operator for efficient comparison with other `ISteppedIntStream` instances.







Sequences or streams of integers are fundamental to a vast number of algorithms. They are constantly used for:

- Controlling the number of repetitions in loops.

- Accessing elements within ordered data structures (like Lists, Tuples, and Strings) by their numerical index.

- Generating series of numbers for mathematical calculations, simulations, or tests.

- Defining ranges for operations like slicing, which extracts a portion of a larger sequence.

Because of their frequent use and critical importance in algorithmic expression, AlgoDraft provides specialized built-in types and convenient literal syntaxes designed specifically for defining and generating **streams of integers**. This approach offers both expressive power for the designer and enhanced clarity within the pseudocode.

At the heart of these mechanisms is the **`IIntegerStream`** interface. This key built-in interface in AlgoDraft serves as the common contract for any type specifically designed to produce a stream of integer values.

**Syntax**:

```
INTERFACE IIntegerStream INHERITS IIterable, IEquatable :=
    // This interface simply marks types that can produce a stream of integers
    // and can be compared for equivalence based on the stream they produce.
    // OPERATOR ITERATOR OF this -> Iterator<Integer> is inherited (IIterable would be generic)
    // OPERATOR this = other -> Boolean is inherited
ENDINTERFACE
```

`IIntegerStream` guarantees that:

- It `INHERITS IIterable`: Any `IIntegerStream` can produce an `Iterator<Integer>` (using `OPERATOR ITERATOR OF this`) and can thus be used directly in `FOR EACH` loops to yield integers one by one.

- It `INHERITS IEquatable`: Instances of types implementing `IIntegerStream` can be compared for equality (=, !=, ≠). For these integer stream generators, equality is defined behaviorally: two instances are considered equal if and only if they produce the **exact same finite or infinite stream of integers**, regardless of how that stream was specified. This means an implementation should be able to check equality against another implementation and returns `TRUE` if they both describe the same stream of integers.

AlgoDraft provides two primary built-in types that implement `IIntegerStream`, each with its own distinct literal syntax for creation:

1. **Interval**: This type defines an integer stream based on a **start boundary, a stop boundary, and an optional step**. It is ideal when the beginning and end points of the desired integer range are the primary defining factors.

2. **Count**: This type defines an integer stream based on a **fixed number of items to generate, a single anchor point (either a starting value `FROM` or an ending value `TO`), and an optional step**. It is best suited when the quantity of integers in the stream is the main specification.

Both `Interval` and `Count` objects are immutable once created. The following subsections will delve into the specific literal syntax for each type, the detailed rules governing how they generate their respective integer streams (including handling of boundaries, steps, defaults, and error conditions), and practical examples of their use in iteration and as specifications for slicing operations. Understanding these will allow you to precisely control integer sequence generation in your AlgoDraft designs.
