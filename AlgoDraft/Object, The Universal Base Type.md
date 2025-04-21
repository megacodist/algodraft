---
status: Writing
---
In AlgoDraft, every entity you work with—be it a [[Numeric Data Type|number]], [[Strings|a piece of text]], or [[Data Structures|a more complex data structure]]—represents a specific **value**. To provide a unified foundation for this value-based system, AlgoDraft defines a universal base type named `Object`.

**Definition:** `Object` is the ultimate ancestor in the AlgoDraft type hierarchy. Every other data type, whether built-in (like [[Integer Basic Type|Integer]], [[Real Basic Type|Real]], [[Complex Basic Type|Complex]], String) or user-defined, directly or indirectly inherits from `Object`.

**Conceptual Role:** You can think of `Object` as representing the abstract concept of "any value" within the AlgoDraft system. If a variable is declared to be of type `Object`, it can hold a value of any specific type (an Integer, a Real, a custom List, etc.). This property is fundamental for writing generic algorithms and data structures that can operate on diverse types of data.
# Universal Value Operations
A core principle in AlgoDraft is that some operations must be anchored at the very root of the type system, the `Object` type, to enforce them universally.

Crucially, these operations are performed by **value**, because we are concerned with what a value represents, not how or where it might be stored (which are implementation details abstracted away by AlgoDraft).

While `Object` mandates the existence of these operations for all types, `Object` itself is too abstract to define how they works for every possible value; the specific rules are defined by each concrete data type:
## Equality/Inequality Operators
These binary operators perform a check, returning a [[Boolean Basic Type|Boolean value]], to determine if its two operands are considered equal or unequal.

- **`a = b` (Equality):** Evaluates to `TRUE` if the value represented by `a` is mathematically or logically equivalent to the value represented by `b`. Otherwise, it evaluates to `FALSE`.
- **`a != b` and `a ≠ b` (Inequality):** Always evaluates to the logical negation of the equality comparison. That is, `a != b` is strictly equivalent to `NOT (a == b)`.