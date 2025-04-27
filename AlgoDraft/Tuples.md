---
status: Writing
---
The essence of a tuple is a **linear, heterogeneous, immutable, iterable, and duplicable** data structure in AlgoDraft:

- **Linear**: Tuples are organized as a strict **sequence** of elements, forming a clear "chain" where:
    
    - Each element has a **specific position** (first, second, etc.).
        
    - **Predecessor** and **successor** relationships are established by order.
        
    - Elements can be accessed by **zero-based index** (0, 1, 2, …).
        
- **Heterogeneous**: Tuples can contain elements of **different types**. For example, a tuple might hold an integer, a string, and a Boolean side by side without restriction.
    
- **Immutable**: Once created, the contents of a tuple **cannot be changed**. No insertion, deletion, or modification of elements is allowed. Any "change" requires constructing a new tuple.
    
- **Iterable**: Tuples support iteration in the **natural, defined order** of their elements, from the first to the last. Constructs like `FOR EACH` are applicable and follow the tuple’s inherent sequence.
    
- **Duplicable**: Duplicate values are allowed in a tuple. The same element can appear multiple times in different positions without conflict.

They are used to represent a fixed collection of related items where order matters, types may differ, and mutability is sacrificed for **predictability, consistency, and conceptual integrity**.

