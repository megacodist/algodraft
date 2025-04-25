---
status: Draft
---
**Duplicability** in AlgoDraft describes whether a data structure **enforces distinctness** among its elements or allows **repetition**. This characteristic helps define how the structure organizes and interprets its contents, especially in contexts like searching, indexing, or identity-based access.

# Duplicable
A structure is **duplicable** if it **permits repeated elements**. This includes values, characters, keys, or any other units the structure stores.

**Characteristics**:
- Preserves **insertion frequency** and **order**.
- Useful for **accumulating**, **counting**, or **maintaining history**.
- Common in Lists, Tuples, Queues, Stacks, and Strings.
- **Mapping values** are duplicable (e.g., different keys can share the same value).
# Non-duplicable
A structure is **non-duplicable** if it **prevents duplicates** — all stored elements (or identifiers) must be distinct.

**Characteristics**:
- Ensures that each element (or key or field) appears only once.
- Useful for **membership testing**, **identity mapping**, or **eliminating redundancy**.
- Found in Sets, Records (field names), and **Mapping keys**.

