---
status: Draft
---
**Iterability** in AlgoDraft describes whether a data structure supports uniform traversal over its elements using the built-in `FOR EACH` construct or similar iteration mechanisms.

In AlgoDraft, **iterability is not an intrinsic property** of a data structure — it is a _capability_ granted by implementing the `IIterable` interface.

# **Iterable**

A data structure is **iterable** if it explicitly **implements the `IIterable` interface**. This allows AlgoDraft’s control structures like `FOR EACH` to iterate over the structure, regardless of its internal complexity.

The iteration strategy guaranteed by AlgoDraft is as follow:

- A `List`, `Tuple` or `String` use _index order_.

- A `Set` uses _no deterministic order_.

- Although `Mapping` is iterable, it offers no established order for keys, values, or key-value pairs iterations.

- A `Stack` reflects _LIFO order_.

- A `Queue` exhibits _FIFO order_.

# **Non-Iterable**

A data structure is **non-iterable** if it does **not** implement the `IIterable` interface.

- This does **not** mean its elements are inaccessible — only that **they cannot be iterated over out of the box** .

- Custom iteration logic can still be written manually or by defining a derived type that implements `IIterable` interface.

- This separation promotes clarity: **"traversability" is optional and composable**, not forced upon every structure.

Inherently non-iterable data structures in AlgoDraft are:

- **Records**: Fields are accessed by name, not by traversal.

- **Trees and Graphs**: Traversal strategies (like DFS, BFS) **exist**, but must be explicitly invoked or implemented — AlgoDraft does not assume a single, inherent way to iterate over them.

**Example**:

```
CLASS Tree
    // Non-iterable by default
ENDCLASS

CLASS InOrderTree INHERITS Tree, IMPLEMENTS IIterable
    // Defines ITERATOR OF in in-order sequence
ENDCLASS
```


