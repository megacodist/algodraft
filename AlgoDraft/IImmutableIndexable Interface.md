---
status: Draft
---
The `IImmutableIndexable` interface in AlgoDraft defines a fundamental contract for data structures whose elements can be accessed directly for **reading only** using their numerical position, or index. It signifies that the collection is not only ordered and has a defined size but also allows for the retrieval of individual elements if their specific position within the sequence is known.

A key characteristic of this interface is its **read-only nature** concerning indexed access. While you can retrieve an element at a specific index, you cannot modify it through this interface; operations that change elements by index are governed by different interfaces, such as `IMutableIndexable`.

`IImmutableIndexable` builds upon the `IContainer` interface. Consequently, any type that implements `IImmutableIndexable` is also an `IContainer` and thus inherently provides the capabilities defined by `IContainer` and its ancestor, `IIterable`:

- The ability to be iterated over (via `OPERATOR ITERATOR OF this`).

- Membership testing (via `OPERATOR element IN this`).

- A way to get its total number of elements (via `OPERATOR LENGTH OF this`).

Several types in AlgoDraft implement this interface, some directly and others indirectly:

- **Direct Implementers:** AlgoDraft's `Stack<T>` and `Queue<T>` types are designed to implement `IImmutableIndexable`. This allows for read-only inspection of their elements by index, offering more visibility than traditional abstract data type (ADT) definitions, alongside their standard stack and queue operations.

- **Indirect Implementers:** Types like `Tuple` and `String` implement `IImmutableIndexable` by virtue of implementing the more specialized `IImmutableSequence` interface, which itself inherits from `IImmutableIndexable`. Similarly, mutable collections like `List<T>` will also support these read-only indexed operations, typically by implementing `IMutableIndexable` which would also inherit from `IImmutableIndexable`.

**Syntax:**

The formal definition of the `IImmutableIndexable` interface in AlgoDraft is as follows:

```
// INTERFACE IIterable :=
//     OPERATOR ITERATOR OF this -> Iterator ENDOPERATOR
// ENDINTERFACE
//
// INTERFACE IContainer INHERITS IIterable :=
//     OPERATOR (element AS ANY) IN this -> Boolean ENDOPERATOR
//     OPERATOR LENGTH OF this -> Integer ENDOPERATOR
// ENDINTERFACE

INTERFACE IImmutableIndexable INHERITS IContainer :=
    // Operator for read-only indexed access to an element.
    // Returns ANY to accommodate heterogeneous collections (like Tuple which implements this
    // via IImmutableSequence) or collections of ANY (like Stack<ANY> or Queue<ANY>).
    // Concrete implementations for specific, homogeneous types (e.g., String returns Char,
    // or a Queue<MyItem> returns MyItem) would effectively provide a more
    // specific return type based on their nature.
    OPERATOR this[index AS Integer] -> ANY ENDOPERATOR

    // Inherited from IContainer (and its ancestor IIterable):
    // OPERATOR (element AS ANY) IN this -> Boolean ENDOPERATOR
    // OPERATOR LENGTH OF this -> Integer ENDOPERATOR
    // OPERATOR ITERATOR OF this -> Iterator ENDOPERATOR
ENDINTERFACE
```

- **`OPERATOR this[index AS Integer] -> ANY`**:
    
    - This is the **indexer** or **subscript operator**, specifically for read-only access.
    
    - It guarantees that an implementing type will provide a mechanism to retrieve an element based on its zero-based index, which must be an Integer.
    
    - The return type `ANY` in the interface definition offers maximum generality. This allows `IImmutableIndexable` to be implemented by heterogeneous collections like `Tuple` (via `IImmutableSequence`) or by generic collections like `Stack<ANY>`. When a concrete, homogeneous type like `String` implements this (via `IImmutableSequence`), its `this[index]` operator will effectively return `Char`. Similarly, a `Queue<MySpecificType>` would return `MySpecificType`.
    
    - **Error Condition:** Attempting to access an index that is outside the valid range (i.e., less than 0 or greater than or equal to `LENGTH OF this`) **must result in an error** being raised by the implementing type.

# Using Operators

## The Newly Added Operator

The primary capability introduced by `IImmutableIndexable` is the ability to retrieve elements using the subscript `[]` operator.

**Syntax:**

```
value_variable <- collection_variable[index_expression]
```

**Semantics:**

- `collection_variable` must be an instance of a type that implements `IImmutableIndexable`.

- `index_expression` must evaluate to a non-negative Integer.

- The operation retrieves the element located at the specified zero-based position within `collection_variable`.

- This access is strictly **read-only** through the `IImmutableIndexable` contract.

**Examples:**

```
// Example with Tuple (implements IImmutableIndexable indirectly via IImmutableSequence)
pointInfo AS Tuple<Integer, String> <- (7, "Alpha")
id AS Integer <- pointInfo[0]      // id becomes 7
label AS String <- pointInfo[1]    // label becomes "Alpha"
NOTIFY {{Point @label has ID @id}}

// Example with String (implements IImmutableIndexable indirectly via IImmutableSequence)
productCode AS String <- "XYZ123"
firstDigit AS Char <- productCode[3] // firstDigit becomes '1'
NOTIFY {{The first digit of product code @productCode is @firstDigit}}
```

## Inherited Operations from `IContainer`

Since `IImmutableIndexable INHERITS IContainer`, all objects that are `IImmutableIndexable` also fully support the operations defined by `IContainer` (which itself inherits from `IIterable`):

- **`LENGTH OF this`**: To get the number of elements.

- **`element IN this`**: To check for membership.

- **`ITERATOR OF this`**: To get an iterator, enabling their use in `FOR EACH` loops.

**Example**:

```
// dataSequence could be a Tuple, String, or an instance of Stack or Queue
// that implements IImmutableIndexable.
dataSequence AS IImmutableIndexable <- ("A", 1, TRUE) // Example: A Tuple assigned to the interface type

sequenceLength AS Integer <- LENGTH OF dataSequence
NOTIFY {{The sequence @dataSequence contains @sequenceLength items}}

searchTerm AS ANY <- 1
IF searchTerm IN dataSequence THEN
    NOTIFY {{The term @searchTerm is present in @dataSequence}}
ENDIF

NOTIFY {{Iterating through @dataSequence:}}
FOR EACH item IN dataSequence DO
    NOTIFY {{Item: @item}}
ENDFOR
```

# Implementations in AlgoDraft

In AlgoDraft, several types provide read-only indexed access by implementing `IImmutableIndexable`, either directly or indirectly:

- **Direct Implementers:**
    
    - `Stack<T>`: AlgoDraft's `Stack` class is designed to be inspectable, allowing read-only access to its elements by index in addition to its LIFO operations.
    
    - `Queue<T>`: Similarly, AlgoDraft's `Queue` class allows read-only indexed access, complementing its FIFO operations.

- **Indirect Implementers** (by implementing an interface, like `IImmutableSequence`, which itself inherits `IImmutableIndexable`):
    
    - `Tuple<T1, T2, ...>` (via `IImmutableSequence`)
    
    - `String` (as an immutable sequence of `Char`, via `IImmutableSequence`)

- **Mutable Types** (by implementing an interface like `IMutableIndexable`, which would inherit `IImmutableIndexable` for its read capabilities):
    
    - `List<T>`

Any user-defined `CLASS` that represents an ordered collection whose elements can be read by their position should also implement this interface.

# Benefits of IImmutableIndexable

- **Standardized Read-Only Positional Access:** Provides a common and consistent way (the `[]` operator) to read elements by their position from a wide variety of ordered collection types.

- **Inspectability of Collections:** Crucially, this interface makes collections like `Stack` and `Queue` more inspectable in AlgoDraft than their traditional ADT counterparts. This can be highly beneficial for algorithm design, debugging, and for certain types of processing where read-only access to internal elements is needed without violating the primary interaction model (push/pop, enqueue/dequeue).

- **Safety Through Immutability Contract:** The "Immutable" aspect of the name clearly signals that this interface guarantees only read access. Operations that modify the collection at an index would belong to a different interface (e.g., `IMutableIndexable`).

- **Foundation for Other Interfaces and Features:** It serves as an essential building block for more specialized sequence interfaces (like `IImmutableSequence` which adds concatenation, or `IMutableIndexable` which adds write capabilities) and potentially for language-level features that require positional read access.

The `IImmutableIndexable` interface extends `IContainer` by adding the contract for **read-only access** to elements using their numerical index via the `[]` operator. It is a key interface for types like AlgoDraft's `Stack` and `Queue` to provide enhanced inspectability, and it forms a foundational layer for other sequence types such as `Tuple`, `String`, and `List` to build upon for their read-access capabilities. This interface promotes a clear distinction between readable and modifiable indexed access within AlgoDraft's type system.
