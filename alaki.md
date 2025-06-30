---
status: Draft
---
In AlgoDraft, an **`Interval`** is a fundamental, built-in immutable data type. It is inspired by *mathematical intervals*, which typically define continuous ranges. However, AlgoDraft's `Interval` is specifically designed to define and generate **streams of discrete integers** that form an arithmetic progression. Think of an `Interval` object not as a collection that stores numbers, but as a **specification** or a **recipe** for producing a stream of integers on demand.

The **primary purpose** of an `Interval` object is to yield this stream of integers when iterated. This makes `Interval`s essential for two main applications within AlgoDraft:

1.  **Iteration Control:** Generating integer streams for use in `FOR EACH` loops, allowing for precise control over direction and stepping, including potentially infinite streams.

2.  **Slicing Specifications:** Defining the exact range of indices to be used for extracting sub-sequences (slices) from sliceable data structures like `Tuples`, `Strings`, and `Lists`.

An `Interval` object encapsulates a conceptual start boundary, a stop boundary, and a step value, which together define the integer stream it produces. `Interval` objects are **first-class citizens** in AlgoDraft: they can be created using a special literal syntax or via direct instantiation (using `NEW Interval(...)`), assigned to variables, passed to functions, and returned from them.

**Creating `Interval` Objects**

There are two ways to create `Interval` objects: using their rich literal syntax (the most common method) or by direct instantiation using `NEW Interval`.

**Literals (Primary Method)**

This is the most expressive and common way to define an interval.

**Syntax:**
`boundary_start startExpr .. stopExpr [step_clause] boundary_end`

*   `startExpr` and `stopExpr`: Integer expressions defining the range boundaries.
*   `step_clause` (optional): `STEP stepValue` or `Δ stepValue` (where `stepValue` is a non-zero `Integer`).
*   `boundary_start` and `boundary_end`: One of `[` (inclusive) or `(` (exclusive).

*(The detailed explanation of Boundary Specifiers, the `STEP`/`Δ` Clause, Default Step Logic, Iteration Direction Error Conditions, Same-Ended Literals, and Open-Ended Literals from your draft is excellent and can remain as is.)*

**Examples of Interval Literals:**
```algodraft
// Standard Ranges
int_0_to_4 AS Interval <- [0 .. 5)            // Stream: 0, 1, 2, 3, 4
int_1_to_9_odds AS Interval <- [1 .. 10 STEP 2] // Stream: 1, 3, 5, 7, 9
int_4_down_to_0 AS Interval <- (5 .. 0 Δ -1]   // Stream: 4, 3, 2, 1, 0
int_5_down_to_2 AS Interval <- [5 .. 1)            // Stream: 5, 4, 3, 2 (step -1 default)

// Same-Ended
single_7 AS Interval <- [7 .. 7]             // Stream: 7
empty_A AS Interval <- [7 .. 7)              // Stream: (empty)

// Open-Ended (Conceptual Defaults for direct iteration)
upTo_3_exclusive AS Interval <- [ .. 3)       // Stream: 0, 1, 2
from_neg2_onwards AS Interval <- [-2 .. )      // Stream: -2, -1, 0, 1, 2, ... (potentially infinite)

// Error Examples:
// err_step_zero AS Interval <- [1 .. 5 STEP 0)  // ERROR: step cannot be zero
// err_dir_neg AS Interval <- [1 .. 5 STEP -1) // ERROR: start < stop but step is negative
// err_dir_pos AS Interval <- [5 .. 1 STEP 1)  // ERROR: start > stop but step is positive
```

**Direct Instantiation using `NEW Interval()`**

While literals are common, `Interval` objects can also be instantiated directly. This is useful if the start, stop, or step values are computed dynamically.

**Conceptual Definition and Constructor:**
`Interval` is a built-in immutable type that `IMPLEMENTS ISteppedIntStream`. Its constructor validates the consistency of the defining parameters.

```pseudocode
// This is a conceptual view of the built-in Interval type's constructor.
// Users interact with it via `NEW Interval(...)`.
// BUILTIN_TYPE Interval IMPLEMENTS ISteppedIntStream :=
//     ... internal attributes for start, stop, step, and boundary inclusivity ...

    METHOD NEW(
            start AS Integer,
            stop AS Integer OR INFINITY,
            step AS Integer OR NULL <- NULL,
            // Additional parameters for boundary inclusivity could be here,
            // or derived from how the constructor is called.
            // For simplicity, we assume the constructor takes effective start/stop values.
        ) :=
        // The constructor performs the same validation logic as the literal parser:
        // 1. Determine effective step (handling NULL default based on start/stop relation).
        // 2. Raise ERROR if step is 0.
        // 3. Raise ERROR if step direction is inconsistent with start-to-stop direction for finite ranges.
        // 4. Initialize the internal state of the Interval object.
    ENDMETHOD

// ENDTYPE
```

**Example of Direct Instantiation:**
```algodraft
limit AS Integer <- 10
customRange AS Interval <- NEW Interval(start <- 1, stop <- limit, step <- 3)
// customRange will yield the stream: 1, 4, 7 (assuming exclusive stop)

reverseRange AS Interval <- NEW Interval(start <- 5, stop <- 0) // step defaults to -1
// reverseRange will yield stream: 5, 4, 3, 2, 1, 0 (assuming inclusive stop)
```

**Characteristics of `Interval` Objects**

1.  **Immutability:**
    `Interval` objects are immutable. Once created, their defining characteristics cannot be changed.

2.  **Interface Conformance (`ISteppedIntStream`):**
    `Interval` **implements `ISteppedIntStream`**. This is a powerful contract which means an `Interval` provides:
    *   **Iterability:** It implements `IIterable<Integer>` (via `IIntegerStream`), so it yields a stream of `Integer`s when used in a `FOR EACH` loop.
        ```algodraft
        NOTIFY {{Iterating interval [1 .. 3]:}}
        FOR EACH num IN [1 .. 3] DO // Yields stream 1, 2, 3
            NOTIFY {{Value: @num}}
        ENDFOR
        ```
    *   **Equatability:** It implements `IEquatable` (via `IIntegerStream`). See details below.
    *   **Stepped Stream Properties:** It provides the methods from `ISteppedIntStream` (`GetFirst()`, `GetLast()`, `GetStep()`, `GetCount()`), which are derived from its start, stop, and step parameters.

3.  **Equatability (`=` operator):**
    *   As an implementer of `ISteppedIntStream`, `Interval` uses an efficient, parameter-based equality check when compared with another `ISteppedIntStream` (like another `Interval` or a `Count` object). It determines equality by comparing the fundamental properties of the arithmetic progressions they represent (effective start, step, and end/count), rather than by iterating through the entire stream.
    *   As an implementer of `IIntegerStream`, it can also be conceptually compared to any other `IIntegerStream`. Equality is ultimately defined by whether they **generate the exact same finite or infinite stream of integers.**
    ```algodraft
    iv1 AS Interval <- [0 .. 3) // Stream: 0, 1, 2
    iv2 AS Interval <- [0 .. 2] // Stream: 0, 1, 2
    countA AS Count <- [COUNT 3 FROM 0] // Stream: 0, 1, 2

    IF iv1 = iv2 THEN
        NOTIFY {{Intervals @iv1 and @iv2 are equivalent}}
    ENDIF
    IF iv1 = countA THEN
        NOTIFY {{Interval @iv1 and Count @countA are equivalent as they produce the same integer stream}}
    ENDIF
    ```

**Use Cases**

The primary role of an `Interval` is to specify a boundary-defined stream of integers.

1.  **Generating Integer Streams for Iteration:**
    Used with `FOR EACH` to control loops that require a specific start and end point.
    ```algodraft
    NOTIFY {{Processing scores from 90 to 100:}}
    FOR EACH score IN [90 .. 100] DO
        NOTIFY {{Checking score: @score}}
    ENDFOR
    ```

2.  **Specifying Slices for Sliceable Sequences:**
    `Interval` objects are the primary tool for defining slices. They are used within the subscript operator `[]` of types that implement interfaces like `IImmutableSliceable` or `IMutableSliceable` (e.g., `Tuple`, `String`, `List`).
    ```algodraft
    sourceText AS String <- "Specification"
    sliceSpecifier AS Interval <- [0 .. 8) // Specifies indices 0 through 7
    // The String's slicing operation uses this Interval:
    result AS String <- sourceText[sliceSpecifier] // result will be "Specific"
    NOTIFY {{Applying interval @sliceSpecifier to @sourceText yields @result}}
    ```