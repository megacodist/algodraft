---
status: Draft
---
In AlgoDraft, data structures are fundamental ways to organize, manage, and store data effectively. They are built using basic data types (like `Integer`, `String`, `Boolean`) and provide a blueprint for how data relates and how it can be accessed or manipulated.

To better understand and choose the right data structure for a task, we categorize them based on several key properties or characteristics:

* **Homogeneity**: Does the structure typically hold elements of the same data type (**homogeneous**) or can it hold elements of different types (**heterogeneous**)? Generics should be used to emphasize homogeneity `$list <- CREATE List<Integer>`.
* **Sequentiality**: Do the elements have a defined order or follow a specific processing sequence (like First-In-First-Out)? Can you predictably move from one element to the next logical one in a sequence? (Yes/No). Structures where elements don't have a defined positional or processing order are **non-sequential**.
* **Linearity**: Do the elements form a single, non-branching sequence? (Yes/No). In a linear structure, each element (except ends) connects to at most one predecessor and one successor. Trees and Graphs are examples of non-linear structures. All linear structures are inherently sequential.
* **Mutability**: Can the structure be changed after creation? Can elements be added, removed, or modified (Mutable)? Or is the structure fixed once created (Immutable)?
* **Iterability**: Is it possible to systematically visit or process each element contained within the structure (e.g., using a `FOR EACH` loop)? (Yes/No). This applies even to non-linear structures, though the traversal method might be more complex (like Depth-First or Breadth-First). Structures like Records, which group named fields, are typically not considered iterable in this sense (you access fields by name, not by iterating over values as members of a collection).
* **Uniqueness**: Does the structure enforce uniqueness for its elements or in some way? (e.g., sets contain unique elements, mappings have unique keys).

# **AlgoDraft Data Structure Characteristics**

| Data Structure | Sequentiality                   | Linearity             | Homogeneity                        | Mutability                                             | Iterability | Uniqueness                                            | Primary Use Case                                      |
| :------------- | :------------------------------ | :-------------------- | :--------------------------------- | :----------------------------------------------------- | :---------- | :---------------------------------------------------- | :---------------------------------------------------- |
| **Tuple**      | Yes                             | Yes                   | **Not Enforced²**                  | Immutable                                              | Yes         | Duplicates Allowed                                    | Grouping a fixed number of related, ordered items.    |
| **String**     | Yes (Sequence of chars)         | Yes                   | **Yes (Characters)**               | **Immutable**                                          | Yes         | Duplicates Allowed (Characters)                       | Representing textual data.                            |
| **List**       | Yes                             | Yes                   | **Not Enforced²**                  | Mutable                                                | Yes         | Duplicates Allowed                                    | Storing sequences where order matters, random access. |
| **Set**        | No                              | No                    | **Not Enforced²**                  | Mutable                                                | Yes         | **Elements Unique**                                   | Storing unique items, checking membership quickly.    |
| **Mapping**    | **Not Enforced³**               | No                    | **Not Enforced²** (Values)         | Mutable                                                | Yes⁴        | **Keys Unique**, Values can duplicate                 | Associating unique keys with arbitrary values.        |
| **Stack**      | Yes (LIFO - Last-In First-Out)  | Yes                   | **Not Enforced²**                  | Mutable                                                | Yes⁵        | Duplicates Allowed                                    | Processing items in reverse order of arrival.         |
| **Queue**      | Yes (FIFO - First-In First-Out) | Yes                   | **Not Enforced²**                  | Mutable                                                | Yes⁵        | Duplicates Allowed                                    | Processing items in order of arrival.                 |
| **Record**     | No (Fields have names)          | No                    | **Not Enforced²**                  | Fields names/number fixed; field values often mutable⁶ | No⁷         | **Field Names Unique**, Values can duplicate          | Grouping related data under specific names.           |
| **Tree**       | No (Hierarchical)               | **No (Hierarchical)** | **Not Enforced²** (Node Data)      | Varies (Structure/Nodes can be mutable)                | Yes⁸        | Varies (Depends on specific tree type/rules)          | Representing hierarchical relationships.              |
| **Graph**      | No (Network)                    | **No (Network)**      | **Not Enforced²** (Node/Edge Data) | Varies (Structure/Nodes/Edges can be mutable)          | Yes⁸        | Varies (Nodes/Edges might be unique depending on use) | Representing complex relationships and networks.      |

**Footnotes:**

² **Homogeneity:** For structures marked `Not Enforced`, homogeneity is not strictly required by AlgoDraft. Instances can contain elements of a single type (homogeneous) or mixed types (heterogeneous), depending on usage. This contrasts with `String`.

³ **Sequentiality:** For Mappings, sequentiality (order of keys/elements during iteration) is `Not Enforced` in AlgoDraft. While some underlying implementations might preserve order (e.g., insertion order), it should not be relied upon by default in algorithms unless explicitly stated for a specific AlgoDraft Mapping type.

⁴ **Iterability (Mapping):** Mappings are iterable, typically allowing iteration over keys, values, or key-value pairs. The iteration order is subject to footnote 3.

⁵ **Iterability (Stack/Queue):** Stacks and Queues are iterable, though primary interaction is often via `Push/Pop` or `Enqueue/Dequeue`. Iteration usually reflects the processing order (LIFO for Stacks, FIFO for Queues).

⁶ **Mutability (Record):** The *structure* of a Record (its fields and their names) is fixed after declaration, but the *values* held in those fields are typically mutable unless the field type itself is immutable (like a String or Tuple in AlgoDraft).

⁷ **Iterability (Record):** Records are not typically iterated over like collections. You access fields by name (`myRecord.fieldName`). Mechanisms might exist to reflect on fields, but that's different from iterating over contained elements.

⁸ **Iterability (Tree/Graph):** Trees and Graphs are iterable using specific traversal algorithms (e.g., Depth-First Search (DFS), Breadth-First Search (BFS)) which define the order elements (nodes/edges) are visited.


# **Why This Matters in AlgoDraft**

Understanding these categories helps you write better pseudocode:

*   **Iterability:** Determines if you can use constructs like `FOR EACH element IN structure`.
*   **Sequentiality/Linearity:** Influences how you think about accessing elements (e.g., by index `list[i]`, by position `stack.Pop()`, or by relationships `node.LeftChild`).
*   **Mutability:** Tells you whether you can modify the structure in place or if operations create new structures.
*   **Non-Iterable Structures (like Record):** Require different access patterns, typically using dot notation (`record.field`).

By choosing a data structure whose properties match your needs, you can express your algorithms more clearly and efficiently in AlgoDraft.