---
status: Draft
---
In AlgoDraft, any data structure that holds ordered collections of elements is **indexable**. This fundamental property means their elements can be accessed using a numerical position or index. All such indexable sequences in AlgoDraft begin by fulfilling the contract of `IContainer`, which provides capabilities like determining `LENGTH OF`, checking membership with `IN`, and iteration via `ITERATOR OF` this (inherited from `IIterable`).

Beyond these basic container properties, indexable sequences are further distinguished by two key characteristics, leading to four primary categories that define how they can be accessed and manipulated:

1. **Index Mutability:** Can elements at a specific index be changed (i.e., can a new value be written to that position)?

2. **Sliceability:** Can a new sequence of the same fundamental type be created by extracting a portion (a "slice") of the original, using an Interval object to specify the range?

This section will explore these four categories by defining the core interfaces that model their contracts: `IImmutableIndexable`, `IMutableIndexable`, `IImmutableSliceable`, and `IMutableSliceable`. We will focus on the unique capabilities each interface adds, emphasizing their similarities and clarifying the boundaries between them.

**Note:**

Operations like concatenation (+) are not part of these general indexable sequence interfaces. Instead, concatenation, appending, removing etc. are specific operators and/or methods provided by concrete sequence types like `Tuple`, `String`, and `List`.

# The Foundational Interfaces (Recap)

Before diving into the specific categories of indexable sequences, recall these foundational interfaces that underpin them:

- **`IIterable`**: Guarantees the `OPERATOR ITERATOR OF this -> Iterator`, enabling use in `FOR EACH` loops.

- **`IContainer` (which `INHERITS IIterable`):** Guarantees `OPERATOR LENGTH OF this -> Integer` and `OPERATOR (element AS ANY) IN this -> Boolean`. All indexable sequences are, at their core, containers.

All subsequent indexable sequence interfaces will build upon `IContainer`.

# The Four Categories of Indexables

## `IImmutableIndexable`: Read-Only Positional Access

This interface represents a contract for ordered collections where elements can be retrieved by index, but not modified through that index. It includes all `IContainer` capabilities.

**Syntax:**

```
INTERFACE IImmutableIndexable INHERITS IContainer :=
    // Adds read-only indexed access
    OPERATOR this[index AS Integer] -> ANY ENDOPERATOR
    // Inherits: LENGTH OF, IN, ITERATOR OF (from IContainer)
ENDINTERFACE
```

**Semantics:**

This is the **indexer** or **subscript operator** for read-only access. It retrieves the element at the specified zero-based index. The interface declares a return type of `ANY` to accommodate potentially heterogeneous sequences; concrete implementing types will effectively return their specific element type (e.g., `String` returns `Char`). An error **must be raised** if index is out of the valid interval of `[0 .. (LENGTH OF sequence_object))`.

**Implementations:**

- `Stack<T>` and `Queue<T>` are designed to directly implement `IImmutableIndexable`, allowing read-only inspection of their contents by index, in addition to their standard LIFO/FIFO operations.

- This interface also serves as a base for all other more specialized indexable sequence interfaces.

**Example:**

```
myTaskQueue AS Queue<String> <- NEW Queue()
DO {{Enqueue some strings into @myTaskQueue}}

IF (LENGTH OF myTaskQueue) > 0 THEN
    frontTask AS String <- myTaskQueue[0] // Reading the front element by index
    NOTIFY {{The task currently at the front of @myTaskQueue is @frontTask}}
ENDIF
```

## `IMutableIndexable`: Writable Positional Access

This interface extends `IImmutableIndexable` by adding the crucial capability to modify (write or set) the element at a specific index. So, elements can be both read from and written to by their position.

**Syntax:**

```
INTERFACE IMutableIndexable INHERITS IImmutableIndexable :=
    // Adds write access at a specific index
    OPERATOR this[index AS Integer] <- value AS ANY ENDOPERATOR
    // Inherits: read this[index], LENGTH OF, IN, ITERATOR OF (from IImmutableIndexable & IContainer)
ENDINTERFACE
```

**Semantics:**

This is the **indexer assignment operator**. It replaces the element at the specified zero-based index with a new value. The new value must be type-compatible with what the sequence is defined to hold at that position (e.g., for a `List<String>`, `value` should be a `String`). An error **must be raised** if index is out of the valid interval.

**Implementations:**

- `List<T>` (indirectly, as it will implement `IMutableSliceable` which includes this capability).

**Example:**

```
CONCEPT
	CLASS PixelRowBuffer IMPLEMENTS IMutableIndexable := {{
		Represents a row of pixel color values. The initializer accepts the following
		attributes in their respective order.
		
		size AS Integer,
		defaultColor AS Integer,
		}}
	ENDCLASS
ENDCONCEPT

pixelBuffer AS PixelRowBuffer <- PixelRowBuffer(5, 0)
pixelBuffer[0] <- 255 // Set the first pixel's color value
pixelBuffer[1] <- 128
pixelBuffer[2] <- pixelBuffer[0] - pixelBuffer[1] // Read via inherited indexer, then write
NOTIFY {{Pixel buffer values: @(pixelBuffer[0]), @(pixelBuffer[1]), @(pixelBuffer[2])}}
```

## `IImmutableSliceable`: Immutable Sequences Supporting Slicing

This interface is for immutable sequences that not only allow read-only indexed access but can also produce new immutable sequences of their own type through a slicing operation.

**Syntax:**

```
INTERFACE IImmutableSliceable INHERITS IImmutableIndexable :=
    // Adds slicing capability, returning a new instance of the same type as 'this'.
    OPERATOR this[interval AS Interval] -> (TYPE OF this) ENDOPERATOR
    // Inherits: read this[index], LENGTH OF, IN, ITERATOR OF (from IImmutableIndexable & IContainer)
ENDINTERFACE
```

**Semantics:**

This operator applies an Interval object (which defines start, stop, step, and inclusivity rules based on its literal form like `[a..b)`) to the original sequence. It produces a **new** sequence containing the selected elements. Crucially, this new sequence is of the **same underlying immutable type** as the original (e.g., slicing a `Tuple` yields a new `Tuple`; slicing a `String` yields a new `String`).

**Implementations:**

- `Tuple<T1, T2, ...>`

- `String` (as an immutable sequence of `Char`)

**Example:**

```
message AS String <- "HelloAlgoDraftWorld!"
// Interval literal [5 .. 14) specifies indices 5 through 13
langName AS String <- message[5 .. 14) // Slices "AlgoDraft"
NOTIFY {{Extracted language name is @langName}}

coordinates AS Tuple<Integer> <- (10, 20, 30, 40, 50)
// Interval literal (1 .. 3] specifies indices 2 (exclusive start) through 3 (inclusive end)
middleValues AS Tuple<Integer> <- coordinates[(1 .. 3]] // Results in tuple (30, 40)
NOTIFY {{Middle coordinate values are @middleValues}}
```

## `IMutableSliceable`: The Best of All Worlds (for Sequences)

This interface category represents sequences that offer the most comprehensive set of capabilities: they are indexable for both reading and writing, and they can also be sliced to produce new sequences of their own type.

**Syntax:**

```
INTERFACE IMutableSliceable INHERITS IMutableIndexable, IImmutableSliceable :=
    // This interface now possesses all capabilities from its parents:
    // - Read this[index] (from IImmutableIndexable)
    // - Write this[index] <- value (from IMutableIndexable)
    // - this[interval AS Interval] -> (TYPE OF this) (from IImmutableSliceable)
    // - LENGTH OF, IN, ITERATOR OF (from IContainer via IImmutableIndexable)

    // Concrete classes like List<T> that implement IMutableSliceable
    // will also typically provide additional methods for structural modification
    // (e.g., Add, InsertAt, RemoveAt, Clear). These list-specific methods
    // would be part of the List<T> class definition or a more specific IList interface.
ENDINTERFACE
```

**Implementations:**

- `List<T>` is the prime example of a type that would implement `IMutableSliceable`. When a List is sliced, it produces a new `List`. `List<T>` would also define its own `+` operator for concatenation and operations like adding/appending, inserting, removing, and clearing as part of its class definition.

# Summary

AlgoDraft categorizes indexable sequences based on their support for **index mutability** and **sliceability**:

1. **`IImmutableIndexable`**: Read-only by index. Not sliceable into its own type via the slice operator. (e.g., AlgoDraft's `Stack`, `Queue` for inspection).

2. **`IImmutableSliceable`**: Read-only by index, AND sliceable into new instances of its own type. (e.g., `Tuple`, `String`).

3. **`IMutableIndexable`**: Read AND write by index. Not sliceable into its own type via the slice operator.

4. **`IMutableSliceable`**: Read AND write by index, AND sliceable into new instances of its own type. (e.g., `List<T>`).

All these interfaces inherit from `IContainer`, meaning all indexable sequences provide `LENGTH OF`, `IN`, and `ITERATOR OF` capabilities.

When designing functions that accept sequences, select the interface that demands the least specific set of capabilities required by your function's logic. This maximizes the reusability and flexibility of your algorithmic components. For example:

- If you only need to read elements by index and know the length: accept `IImmutableIndexable`.

- If you need to read, get length, and also create sub-sequences of the same immutable type: accept `IImmutableSliceable`.

- If you need to modify elements in place by index: accept `IMutableIndexable`.

- If you need full mutable random access plus slicing: accept `IMutableSliceable` (or a `List<T>` if list-specific operations like add are also needed).

This clear separation of concerns through interfaces allows for precise and robust algorithm design in AlgoDraft.