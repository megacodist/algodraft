---
status: Planned
---
**Sequentiality** refers to whether a data structure maintains a **specific order** among its elements — an order that is **visible and significant** during access, iteration, or processing.

Sequentiality affects how elements are **added, accessed, and traversed**, and whether their position within the structure matters conceptually.

# Sequential
A structure is **sequential** if it maintains a **defined order of elements**. This order reflects the position of each element relative to others and is preserved during iteration.

**Characteristics**:
- Order is meaningful and **preserved**.
- You can say: _“The second element is...”_ or _“At index 3...”_
- Supports **positional access** (e.g., `myList[2]`).
- Essential in Strings, Lists, Tuples, Queues, and Stacks.

# Non-sequential
A structure is **non-sequential** if it **does not define or guarantee** a meaningful order of elements.

**Characteristics**:
- Order is either **undefined** or **not significant**.
- You cannot refer to “the first element” in a meaningful way.
- Access is based on **membership** (Set), **key lookup** (Mapping), or **field name** (Record).
- Found in Sets, Mappings, and Records.