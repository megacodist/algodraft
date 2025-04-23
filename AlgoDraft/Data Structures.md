---
status: Draft
---
In AlgoDraft, data structures are fundamental ways to organize, manage, and store data effectively. They are built using basic data types (like `Integer`, `String`, `Boolean`) and provide a blueprint for how data relates and how it can be accessed or manipulated.

To better understand and choose the right data structure for a task, we categorize them based on several key properties or characteristics:

*  **Element Topology**: Refers to the _conceptual arrangement_ of elements within a data structure — how they are positioned, related, or connected. It captures the “shape” or internal organization of the data and defines how elements are accessed or navigated in relation to each other.
* * **Sequentiality**: Describes whether the elements have a defined order or follow a specific processing sequence (like First-In-First-Out) and you can predictably move from one element to the next logical one in a sequence. Structures where elements don't have a defined positional or processing order are **non-sequential**.
* * **Homogeneity**: Indicates whether the structure typically hold elements of the same data type (**homogeneous**) or can it hold elements of different types (**heterogeneous**).
* **Mutability**: Can the structure be changed after creation? Can elements be added, removed, or modified (Mutable)? Or is the structure fixed once created (Immutable)?
* **Iterability**: Is it possible to systematically visit or process each element contained within the structure (e.g., using a `FOR EACH` loop)? This applies even to non-linear structures, though the traversal strategies might be more complex (like Depth-First or Breadth-First). Structures like Records, which group named fields, are typically not considered iterable in this sense (you access fields by name, not by iterating over values as members of a collection).
* **Uniqueness**: Indicates whether elements (or certain parts, like keys or field names) must be unique within the structure. This affects membership tests and structural integrity.

# **AlgoDraft Data Structures Characteristics/Categorizations**
| Data Structure | **Element Topology** | Sequentiality                   | Homogeneity                             | Mutability                                             | Iterability | Uniqueness                                            | Primary Use Case                                      |
| -------------- | -------------------- | ------------------------------- | --------------------------------------- | ------------------------------------------------------ | ----------- | ----------------------------------------------------- | ----------------------------------------------------- |
| **Tuple**      | Linear               | Yes                             | **Not Enforced²**                       | Immutable                                              | Yes         | Duplicates Allowed                                    | Grouping a fixed number of related, ordered items.    |
| **String**     | Linear               | Yes (Sequence of chars)         | **Yes (Characters)**                    | **Immutable**                                          | Yes         | Duplicates Allowed (Characters)                       | Representing textual data.                            |
| **List**       | Linear               | Yes                             | **Not Enforced²**                       | Mutable                                                | Yes         | Duplicates Allowed                                    | Storing sequences where order matters, random access. |
| **Set**        | Unstructured         | No                              | **Not Enforced²**                       | Mutable                                                | Yes         | **Elements Unique**                                   | Storing unique items, checking membership quickly.    |
| **Mapping**    | Associative          | **Not Enforced³**               | **Not Enforced²** (for Keys and Values) | Mutable                                                | Yes⁴        | **Keys Unique**, Values can duplicate                 | Associating unique keys with arbitrary values.        |
| **Stack**      | Linear               | Yes (LIFO - Last-In First-Out)  | **Not Enforced²**                       | Mutable                                                | Yes⁵        | Duplicates Allowed                                    | Processing items in reverse order of arrival.         |
| **Queue**      | Linear               | Yes (FIFO - First-In First-Out) | **Not Enforced²**                       | Mutable                                                | Yes⁵        | Duplicates Allowed                                    | Processing items in order of arrival.                 |
| **Record**     | Associative          | No (Fields have names)          | **Not Enforced²**                       | Fields names/number fixed; field values often mutable⁶ | No⁷         | **Field Names Unique**, Values can duplicate          | Grouping related data under specific names.           |
| **Tree**       | Hierarchical         | No (Hierarchical)               | **Not Enforced²** (Node Data)           | Varies (Structure/Nodes can be mutable)                | Yes⁸        | Varies (Depends on specific tree type/rules)          | Representing hierarchical relationships.              |
| **Graph**      | Networked            | No (Network)                    | **Not Enforced²** (Node/Edge Data)      | Varies (Structure/Nodes/Edges can be mutable)          | Yes⁸        | Varies (Nodes/Edges might be unique depending on use) | Representing complex relationships and networks.      |
**Footnotes:**

² **Homogeneity**: For structures marked `Not Enforced`, AlgoDraft does not require elements to share the same type. Mixed or uniform types may be used as appropriate.

³ **Sequentiality (Mapping)**: AlgoDraft does not guarantee order in Mappings. If order is relevant (e.g., by insertion), it must be stated explicitly.

⁴ **Iterability (Mapping)**: Mappings are iterable over keys, values, or key-value pairs. Order is unspecified unless defined.

⁵ **Iterability (Stack/Queue)**: Stacks and Queues allow iteration, but are primarily used via `Push`/`Pop` or `Enqueue`/`Dequeue` operations.

⁶ **Mutability (Record)**: A Record’s structure is fixed after definition, but its field values may be mutable depending on their types.

⁷ **Iterability (Record)**: Records are typically accessed by field name rather than iteration. Field reflection may exist but is not standard iteration.

⁸ **Iterability (Tree/Graph)**: Trees and Graphs are traversed using traversal algorithms (e.g., DFS, BFS); iteration order is defined by the traversal method.


# **Why This Matters in AlgoDraft**

Understanding these categories helps you write better pseudocode:

*   **Iterability:** Determines if you can use constructs like `FOR EACH element IN structure`.
*   **Sequentiality/Linearity:** Influences how you think about accessing elements (e.g., by index `list[i]`, by position `stack.Pop()`, or by relationships `node.LeftChild`).
*   **Mutability:** Tells you whether you can modify the structure in place or if operations create new structures.
*   **Non-Iterable Structures (like Record):** Require different access patterns, typically using dot notation (`record.field`).

By choosing a data structure whose properties match your needs, you can express your algorithms more clearly and efficiently in AlgoDraft.