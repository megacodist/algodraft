---
status: Writing
---
The **`Count`** type is AlgoDraft's second specialized built-in mechanism for generating **streams of integers**. It serves as a powerful complement to the `Interval` type.

While an `Interval` defines a stream by its start and end boundaries, a `Count` object defines a stream by specifying:
1.  A **fixed number of integers** to produce (`COUNT`).
2.  A **single anchor point**—either a **starting value** (`FROM`) or an **ending value** (`TO`).
3.  An optional **step value** (`STEP` or `Δ`).

`Count` is the ideal tool when the *quantity* of integers you need to generate is the primary defining factor of your sequence, or when you need to create an infinite arithmetic progression from a known starting point. Like `Interval` objects, `Count` objects are immutable and are primarily used for iteration control and as specifications for slicing sequences.

**b. The `Count` Built-in Type**

`Count` is an immutable built-in type in AlgoDraft. It implements the **`ISteppedIntStream`** interface, which means it provides all the capabilities of a step-wise integer stream:

*   It `IMPLEMENTS IIterable<Integer>` (via `ISteppedIntStream`), so it yields a stream of `Integer`s when used in a `FOR EACH` loop.
*   It `IMPLEMENTS IEquatable` (via `ISteppedIntStream`), so it can be compared for equality with other `IIntegerStream` objects (like other `Count`s or `Interval`s) based on the stream of integers they produce.
*   It provides the methods of `ISteppedIntStream`: `GetFirst()`, `GetLast()`, `GetStep()`, and `GetCount()`.

Its instances are created using a specific literal syntax. Direct instantiation via `NEW Count(...)` with named parameters for `count`, `from` (or `to`), and `step` is also conceptually supported for programmatic creation.

**c. `Count` Literal Syntax**

All `Count` literals are enclosed in square brackets `[` `]` and **must begin with the `COUNT` keyword**. Exactly one of `FROM startExpr` or `TO endExpr` must be provided after `COUNT countExpr`.

**Form 1 (Count From Start):**
`[COUNT countExpr FROM startExpr [STEP stepExpr | Δ stepExpr]]`
*   **`COUNT countExpr`**: Mandatory. `countExpr` is a non-negative `Integer` or the keyword `INFINITY`. A `COUNT` of `0` yields an empty stream.
*   **`FROM startExpr`**: Mandatory in this form. `startExpr` is an `Integer` specifying the first value in the stream.
*   **`STEP stepExpr | Δ stepExpr`** (Optional): `stepExpr` is a non-zero `Integer`. **Defaults to `1`**.

**Form 2 (Count To End):**
`[COUNT countExpr TO endExpr [STEP stepExpr | Δ stepExpr]]`
*   **`COUNT countExpr`**: Mandatory. `countExpr` is a non-negative `Integer`. **The keyword `INFINITY` is NOT allowed for `countExpr` in the `TO` form**, as one cannot count infinitely *to* a finite endpoint.
*   **`TO endExpr`**: Mandatory in this form. `endExpr` is an `Integer` specifying the *last* value that will be yielded by the stream.
*   **`STEP stepExpr | Δ stepExpr`** (Optional): `stepExpr` is a non-zero `Integer`. **Defaults to `1`**.
*   *The stream's starting value is calculated based on `endExpr`, `countExpr`, and `stepExpr` using the formula: `calculated_start = endExpr - (countExpr - 1) * stepExpr` (this applies if `countExpr > 0`).*

**Error Conditions for `Count` Literals:**
*   `countExpr` (if an integer) is negative.
*   `stepExpr` (if specified) is `0`, unless `countExpr` is `1`. For a stream of one element, any non-zero step is conceptually valid as no "step" is taken, but a step of `0` is still disallowed for consistency to prevent potential infinite loops if `count` were greater than 1.
*   `COUNT INFINITY` is used with the `TO endExpr` form.

**Examples of `Count` Literals:**
```algodraft
// Count From Start
cfs1 AS Count <- [COUNT 5 FROM 0]                // Stream: 0, 1, 2, 3, 4 (STEP 1 default)
cfs2 AS Count <- [COUNT 3 FROM 10 STEP -2]       // Stream: 10, 8, 6
cfs3 AS Count <- [COUNT INFINITY FROM 1]         // Stream: 1, 2, 3, ... (STEP 1 default)
cfs4 AS Count <- [COUNT 0 FROM 100]              // Stream: (empty)
cfs5 AS Count <- [COUNT 1 FROM 50 STEP 0]        // ERROR: step is zero (even for count 1)

// Count To End
cte1 AS Count <- [COUNT 5 TO 4]                  // STEP 1 default. Calculated Start = 0. Stream: 0, 1, 2, 3, 4
cte2 AS Count <- [COUNT 3 TO 10 STEP 2]          // Calculated Start = 6. Stream: 6, 8, 10
cte3 AS Count <- [COUNT 4 TO 1 Δ -3]            // Calculated Start = 10. Stream: 10, 7, 4, 1

// Error Examples for Count Literals
// err_count AS Count <- [COUNT -2 FROM 1]             // ERROR: count cannot be a negative integer
// err_infinity AS Count <- [COUNT INFINITY TO 0]     // ERROR: COUNT INFINITY cannot be used with TO form
```

**d. Characteristics and Usage of `Count` Objects**

`Count` objects share all the characteristics of an `IIntegerStream`, including immutability, iterability, and equatability.

*   **Iterability (`IIterable` via `IIntegerStream`):**
    `Count` objects are primarily used to generate integer streams within `FOR EACH` loops, especially when the number of iterations is known.
    ```algodraft
    NOTIFY {{The first 4 powers of 2 starting from 1:}}
    // COUNT 4 items, FROM 1, STEP is implicit *2 logic
    // A Count object cannot have a dynamic step, so we calculate it inside the loop.
    // For a fixed step:
    NOTIFY {{A sequence of 4 items starting at 100 with step 10:}}
    FOR EACH val IN [COUNT 4 FROM 100 STEP 10] DO // Stream: 100, 110, 120, 130
        NOTIFY {{Value: @val}}
    ENDFOR
    ```

*   **Equatability (`IEquatable` via `IIntegerStream`):**
    As `Count` implements `IIntegerStream`, it can be compared for equality with other `IIntegerStream` instances, including `Interval`s. The comparison is `TRUE` if they produce the exact same stream of integers. AlgoDraft's built-in types will use an efficient, parameter-based comparison where possible.
    ```algodraft
    countA AS Count <- [COUNT 3 FROM 1]       // Stream: 1, 2, 3
    intervalB AS Interval <- [1 .. 3]           // Stream: 1, 2, 3
    countC AS Count <- [COUNT 3 TO 3]         // Stream: 1, 2, 3

    IF countA = intervalB THEN
        NOTIFY {{The Count stream from @countA is equivalent to the Interval stream from @intervalB}}
    ENDIF
    IF countA = countC THEN
        NOTIFY {{The two Count streams from @countA and @countC are equivalent}}
    ENDIF
    ```

*   **Slicing Sequences:**
    `Count` objects can be used as specifications within the subscript operator `[]` of types that implement sliceable interfaces (like `IROSequentiable`). The `Count` object generates the stream of indices that the slicing operation will use.
    ```algodraft
    sourceData AS Tuple<String> <- ("a", "b", "c", "d", "e", "f")
    // Get 3 items starting from index 2
    sliceSpec AS Count <- [COUNT 3 FROM 2] // Generates indices: 2, 3, 4
    subTuple AS Tuple<String> <- sourceData[sliceSpec]
    NOTIFY {{Sub-tuple is @subTuple}} // ("c", "d", "e")
    ```

**e. `Interval` vs. `Count` for Integer Streams**

While both `Interval` and `Count` generate streams of integers and can often be used interchangeably, they are optimized for different ways of thinking about a sequence:

*   Use **`Interval`** when you know the **start and end boundaries** of your desired range. It's ideal for situations like "process all items from index `start` to index `stop`."
*   Use **`Count`** when you know the **quantity of items** you need to generate from or towards a specific anchor point. It's ideal for situations like "do this `N` times," "get the first `N` items," or "get the last `N` items ending at this index."

Choosing the appropriate type makes your algorithm's intent clearer and more self-documenting.