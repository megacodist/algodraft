---
status: Draft
---




In AlgoDraft, an **Interval** is a fundamental, built-in immutable data type. It is inspired by mathematical intervals, which typically define ranges of real numbers. However, AlgoDraft's Interval is specifically designed to define and generate **streams of discrete integers**. Think of an Interval object not as a collection that stores numbers, but as a specification or a recipe for producing a sequence of integers on demand (a stream).

The **primary purpose** of an Interval object is to yield this stream of integers when iterated. This makes Intervals essential for two main applications within AlgoDraft:

1. **Iteration Control:** Generating integer sequences for use in `FOR EACH` loops, allowing for precise control over counting, stepping, and direction (including potentially infinite streams if not otherwise bounded).

2. **Slicing Specifications:** Defining the exact range and step for extracting sub-sequences (slices) from sliceable data structures like `Tuple`s and `String`s.

An Interval object encapsulates a conceptual start point, an end point, a step value, and the rules of inclusivity for its boundaries, all of which determine the stream of integers it can produce.

Interval objects are **first-class citizens** in AlgoDraft: they can be created using a special literal syntax or via a constructor, assigned to variables, passed to functions, and returned from them.

# Creating Interval Objects

There are two ways to create Interval objects: using their rich literal syntax (most common) or by direct instantiation using a constructor.

## Literals (Primary Method)

This is the most expressive and common way to define an interval.

**Syntax:**

```
boundary_start startExpr .. stopExpr [step_clause] boundary_end
```

- `startExpr` and `stopExpr`: Integer expressions defining the range boundaries.

- `step_clause` (optional): `STEP stepValue` or `Δ stepValue` (where `stepValue` is an Integer).

- `boundary_start` and `boundary_end`: One of `[` (inclusive) or `(` (exclusive).

- **Boundary Specifiers (Inclusivity/Exclusivity):**
	
    * The characters `[` and `]` denote **inclusive** boundaries (the value is part of the range).
    
    * The characters `(` and `)` denote **exclusive** boundaries (the value is not part of the range; iteration starts/stops after/before it).  
    
    - `[start .. stop]`: Inclusive start, Inclusive stop.
    
    - `[start .. stop)`: Inclusive start, Exclusive stop (common for 0-based indexing where stop is the count).
    
    - `(start .. stop]`: Exclusive start, Inclusive stop.
    
    - `(start .. stop)`: Exclusive start, Exclusive stop.

- **The `STEP` / `Δ` Clause (Optional):**
    
    - Specifies the increment (`stepValue` > 0) or decrement (`stepValue` < 0) between numbers in the generated stream.
    
    - `STEP 0` or `Δ 0` is invalid and **raises an ERROR**.
    
    - **Default Step Logic:**
        
        - If `STEP` is omitted and `startExpr <= stopExpr` (or is implied by an open-ended notation): step defaults to `1`.
        
        - If `STEP` is omitted and `startExpr > stopExpr` (or is implied by an open-ended notation): step defaults to `-1`.

- **Iteration Direction and Step Consistency (Error Conditions):**
    
    - If `startExpr < stopExpr` (or is implied by an open-ended notation), an explicitly provided `stepValue` **must be positive**. A negative `stepValue` in this case **raises an ERROR**.
    
    - If `startExpr > stopExpr` (or is implied by an open-ended notation), an explicitly provided `stepValue` **must be negative**. A positive `stepValue` in this case **raises an ERROR**.

- **Special Case: `startExpr = stopExpr` Literals:**
    
    - `[a .. a]`: Yields a stream with the single integer `a`.
    
    - `[a .. a), (a .. a], (a .. a)`: All yield an empty stream.

- **Open-Ended Interval Literals:**
    
    - `[ .. stopExpr ... ) or ( .. stopExpr ... )`: The start boundary defaults to `0`.
    
    - `[startExpr .. ... ] or (startExpr .. ... ]`: The stop boundary effectively defaults to a conceptual `INFINITY`. Iterating such an interval directly (e.g., `FOR EACH i IN [0..) DO ...`) will produce an infinite stream unless the loop is broken by other means. When used for slicing a sequence, `INFINITY` is resolved to the sequence's end.
    
    - `[ .. ]` (and its bracket variants): Represents a full conceptual range (e.g., `start <- 0`, `stop <- ∞`, `step <- 1` if iterated directly, or all indices of a sequence if used in slicing).

**Example:**

```
// Standard Ranges
int_0_to_4 AS Interval <- [0 .. 5)            // Stream: 0, 1, 2, 3, 4 (step 1 default)
int_1_to_9_odds AS Interval <- [1 .. 10 STEP 2] // Stream: 1, 3, 5, 7, 9
int_4_down_to_0 AS Interval <- (5 .. 0 Δ -1]   // Stream: 4, 3, 2, 1, 0
int_5_down_to_2 AS Interval <- [5 .. 1)            // Stream: 5, 4, 3, 2 (step -1 default)

// Same-Ended
single_7 AS Interval <- [7 .. 7]             // Stream: 7
empty_A AS Interval <- [7 .. 7)              // Stream: (empty)

// Open-Ended (Conceptual Defaults for direct iteration)
upTo_3_exclusive AS Interval <- [ .. 3)       // Stream: 0, 1, 2
from_neg2_onwards AS Interval <- [-2 .. )      // Stream: -2, -1, 0, 1, 2, ... (potentially infinite)
all_non_negatives AS Interval <- [ .. )      // Stream: 0, 1, 2, ... (potentially infinite)

// Error Examples:
// err_step_zero AS Interval <- [1 .. 5 STEP 0)  // ERROR: step cannot be zero
// err_dir_neg AS Interval <- [1 .. 5 STEP -1) // ERROR: start < stop but step is negative
// err_dir_pos AS Interval <- [5 .. 1 STEP 1)  // ERROR: start > stop but step is positive
```

## Instantiation

While literals are common, Interval objects can also be instantiated directly. This is useful if intervals must be treated like first-class citizens.

**Syntax:**

```
CLASS Interval IMPLEMENTS IIterable, IEquatable :=
    // Internal attributes, normalized from literal or constructor arguments
    start AS Integer,
    stop AS Integer OR ∞,  // Represents the conceptual target, may hold a marker for INFINITY
    step AS Integer,

	METHOD NEW(
	        start AS Integer <- 0,
	        stop AS Integer OR ∞ <- ∞,
	        step AS Integer OR NULL <- NULL,
	        ) :=
	    this.start <- start
	    this.stop <- stop
	    startGreater AS Boolean <- start > stop
	    IF step = NULL THEN
	        this.step <- -1 IF startGreater ELSE 1
	    ELSE IF step = 0 THEN
	        RAISE {{zero as Interval step is invalid}}
	    ELSE
	        MATCH (step > 0, startGreater,) AGAINST
	            CAES (TRUE, TRUE,) THEN
	                RAISE {{positive step and greater start is an invalid conjunction}}
	            CASE (FALSE, FALSE,) THEN
	                RAISE {{negative step and greater stop is an invalid conjunction}}
	            ELSE
	                this.step <- step
	        ENDMATCH
	    ENDIF
	ENDMETHOD

    // OPERATOR ITERATOR OF this -> Iterator<Integer> := ...
    // OPERATOR this = other AS Interval -> Boolean := ...
ENDTYPE
```

The following table summarizes how normalized step is computed by `start`, `stop`, and `step` arguments:

| step argument | start > stop  | start <= stop |
| ------------- | ------------- | ------------- |
| NULL          | -1            | 1             |
| 0             | ERROR         | ERROR         |
| positive      | ERROR         | step argument |
| negative      | step argument | ERROR         |

**Example:**

```
limit AS Integer <- 10
customRange AS Interval <- NEW Interval(start <- 1, stop <- limit, step <- 3)
// customRange will yield the stream: 1, 4, 7

reverseRange AS Interval <- NEW Interval(start <- 5, stop <- 0) // step defaults to -1
// reverseRange will yield stream: 5, 4, 3, 2, 1, 0
```

### Characteristics

1. **Immutability:**
	
	Interval objects are immutable. Once created (either from a literal or via `NEW`), their defining `start`, `stop`, and `step` characteristics cannot be changed.

2. **Iterability:**
	
	`Interval` **implements `IIterable`**. Iterating over an `Interval` object **yields a stream of Integers** according to its defined `start`, `stop`, `step`, and boundary inclusivity/exclusivity rules (as normalized from its creation).
	
	```
	NOTIFY {{Iterating interval [1 .. 3]:}}
	FOR EACH num IN [1 .. 3] DO // Literal [1..3] creates an Interval; yields stream 1, 2, 3
	    NOTIFY {{Value: @num}}
	ENDFOR
	```

3. **Equatability:**
	
	`Interval` **implements `IEquatable`**. Two `Interval` objects are considered equal (`=`) if and only if they **generate the exact same finite stream of integers** when iterated. If both generate infinite streams with the same `start` and `step`, they are also considered equal.

	```
	iv1 AS Interval <- [0 .. 3)          // Stream: 0, 1, 2
	iv2 AS Interval <- [0 .. 2]          // Stream: 0, 1, 2
	iv3 AS Interval <- (0 .. 6 Δ 2)    // Stream: 1, 3, 5
	
	IF iv1 = iv2 THEN
		NOTIFY {{Intervals @iv1 and @iv2 are equivalent because they produce the same integer stream}}
	ENDIF
	
	// Infinite stream comparison
	infinite1 AS Interval <- [0 .. )
	infinite2 AS Interval <- NEW Interval(startVal <- 0, stepVal <- 1) // stopVal defaults to INFINITY
	IF infinite1 = infinite2 THEN
		NOTIFY {{Both @infinite1 and @infinite2 represent the stream 0, 1, 2, ...}}
	ENDIF
	```

# Use Cases

The primary role of an Interval is to specify how to generate a stream of integers. This makes them useful in two main contexts:

1. **Generating Integer Streams for Iteration:** Used with the `FOR EACH` loop to control iterations requiring specific integer ranges, steps, or directions.
	
	```
	NOTIFY {{Counting up by threes:}}
	FOR EACH count IN [0 .. 10 STEP 3) DO // Stream: 0, 3, 6, 9
	    NOTIFY {{Current count: @count}}
	ENDFOR
	
	NOTIFY {{Accessing elements in reverse:}}
	items AS List<String> <- ["A", "B", "C"]
	// Interval (LENGTH OF items .. 0] generates indices: 2, 1, 0
	// This works even if 'items' is empty, as (0 .. 0] produces an empty stream.
	FOR EACH idx IN (LENGTH OF items .. 0] DO
	    NOTIFY {{Item at index @idx is @(items[idx])}}
	ENDFOR
	```

2. **Specifying Slices for Sliceable Sequences:** `Interval` objects (created via literals or `NEW`) are used within the subscript operator `[]` of types that implement `IImmutableSliceable` or `IMutableSliceable` (e.g., `Tuple`, `String`, `List`). The `Interval` tells the slicing operation which indices to include.

	```
	sourceText AS String <- "AlgoDraftIsCool"
	sliceSpecifier AS Interval <- [0 .. 9 Δ 2) // Specifies indices 0, 2, 4, 6, 8
	// The String's slicing operation uses this Interval:
	result AS String <- sourceText[sliceSpecifier] // result will be "AgDat"
	NOTIFY {{Applying @sliceSpecifier to @sourceText yields @result}}
	```
