---
status: Draft
---
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
