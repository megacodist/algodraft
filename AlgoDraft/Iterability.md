---
status: Draft
---
**Iterability** refers to whether a data structure’s elements can be **visited one by one in a defined and repeatable manner**. In AlgoDraft, iterability doesn’t imply how the iteration is implemented, but **whether the concept of “visiting elements” makes sense** for the structure.
# **Iterable**
An iterable structure allows traversal over its elements, typically using **loops**, **traversals**, or **other step-wise mechanisms**. The **order of iteration** may be:

- Well-defined (as in Lists or Strings),
- Algorithmically defined (as in Trees or Graphs),
- Undefined but still supported (as in Sets).
    

**Implications in AlgoDraft:**
- You can use iteration constructs like `FOR EACH` with them.
- Essential for algorithms that **examine**, **process**, or **filter** data.
    

**Examples:**
- Lists, Strings, Tuples (in their natural order)
- Sets (order not guaranteed)
- Mappings (can iterate over keys, values, or key-value pairs)
- Trees and Graphs (through DFS, BFS, etc.)
- Stacks and Queues (though they’re usually processed rather than walked)

# **Non-Iterable**

A non-iterable structure **does not support** element-wise traversal because its elements **aren’t structured for stepping through**. These structures are **accessed directly by name or position**, and not through looping.

**Implications in AlgoDraft:**
- You cannot use `FOR EACH` directly.
- Processing typically involves **direct access by field or method**, not iteration.

**Examples:**
- **Records**: You don’t iterate over fields; you access them by name (e.g., `person.age`)
- Some abstract containers or wrappers without iterable semantics