---
status: Draft
---
In AlgoDraft, data structures are fundamental ways to organize, manage, and store data effectively. They are built using basic data types (like `Integer`, `String`, `Boolean`) and provide a blueprint for how data relates and how it can be accessed or manipulated.

To better understand and choose the right data structure for a task, we categorize them based on several key properties or characteristics:

*  **Elements Topology**: Refers to the _conceptual arrangement_ of elements within a data structure — how they are positioned, related, or connected. It captures the “shape” or internal organization of the data and defines how elements are accessed or navigated in relation to each other.
* * **Homogeneity**: Indicates whether the structure typically hold elements of the same data type (**homogeneous**) or can it hold elements of different types (**heterogeneous**).
* **Mutability**: Can the structure be changed after creation? Can elements be added, removed, or modified (**mutable**)? Or is the structure fixed once created (**immutable**)?
* **Iterability**: Is it possible to systematically visit or process each element contained within the structure (e.g., using a `FOR EACH` loop)? This applies even to non-linear structures, though the traversal strategies might be more complex (like Depth-First or Breadth-First). Structures like Records, which group named fields, are typically not considered iterable in this sense (you access fields by name, not by iterating over values as members of a collection).
* **Duplicability**: Indicates whether elements (or certain parts, like keys or field names) are duplicate within the structure or not. This affects membership tests and structural integrity.

# **AlgoDraft Data Structures Characteristics/Categorizations**

| Data Structure | Elements Topology | Homogeneous | Mutable | Iterable | Duplicable                | Primary Use Case                                      |
| -------------- | ----------------- | ----------- | ------- | -------- | ------------------------- | ----------------------------------------------------- |
| **Tuple**      | Linear            | No          | No      | Yes      | Yes                       | Grouping a fixed number of related, ordered items.    |
| **String**     | Linear            | Yes         | No      | Yes      | Yes                       | Representing textual data.                            |
| **List**       | Linear            | No          | Yes     | Yes      | Yes                       | Storing sequences where order matters, random access. |
| **Set**        | Unstructured      | No          | Yes     | Yes      | No                        | Storing unique items, checking membership quickly.    |
| **Mapping**    | Associative       | No          | Yes     | Yes¹     | Keys: No  <br>Values: Yes | Associating unique keys with arbitrary values.        |
| **Stack**      | Linear            | No          | Yes     | Yes²     | Yes                       | Processing items in reverse order of arrival.         |
| **Queue**      | Linear            | No          | Yes     | Yes²     | Yes                       | Processing items in order of arrival.                 |
| **Record**     | Associative       | No          | Partly³ | No       | Fields: No  Values: Yes   | Grouping related data under specific names.           |
| **Tree**       | Hierarchical      | No          | Varies⁴ | Yes⁵     | Varies⁶                   | Representing hierarchical relationships.              |
| **Graph**      | Networked         | No          | Varies⁴ | Yes⁵     | Varies⁶                   | Representing complex relationships and networks.      |

**Footnotes**

¹ **Mapping Iterability:** Mappings are iterable by keys, values, or key-value pairs. Order is undefined unless specified.

² **Stack/Queue Iterability:** Though their primary interface is procedural (push/pop, enqueue/dequeue), they are iterable in their internal order (LIFO for stacks, FIFO for queues).

³ **Record Mutability:** Field structure is fixed after declaration. Field _values_ are mutable depending on their type.

⁴ **Tree/Graph Mutability:** Trees and graphs may have mutable nodes, edges, and structure — this varies by use.

⁵ **Tree/Graph Iterability:** These are iterable via traversal methods like DFS and BFS, rather than element-by-element iteration.

⁶ **Tree/Graph Duplicability:** Whether elements must be unique depends on the specific variant or constraints of the structure.

# **Why This Matters in AlgoDraft**

Understanding these categories helps you write better pseudocode:

*   **Iterability:** Determines if you can use constructs like `FOR EACH element IN structure`.
*   **Sequentiality/Linearity:** Influences how you think about accessing elements (e.g., by index `list[i]`, by position `stack.Pop()`, or by relationships `node.LeftChild`).
*   **Mutability:** Tells you whether you can modify the structure in place or if operations create new structures.
*   **Non-Iterable Structures (like Record):** Require different access patterns, typically using dot notation (`record.field`).

By choosing a data structure whose properties match your needs, you can express your algorithms more clearly and efficiently in AlgoDraft.