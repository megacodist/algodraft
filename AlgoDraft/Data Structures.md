---
status: Draft
---
In AlgoDraft, data structures are fundamental ways to organize, manage, and store data effectively. They are built using basic data types (like `Integer`, `String`, `Boolean`) and provide a blueprint for how data relates and how it can be accessed or manipulated.

To better understand and choose the right data structure for a task, we categorize them based on several key properties or characteristics:

*  **Elements Topology**: Refers to the _conceptual arrangement_ of elements within a data structure — how they are positioned, related, or connected. It captures the “shape” or internal organization of the data and defines how elements are accessed or navigated in relation to each other.
* **Homogeneity**: Indicates whether the structure typically hold elements of the same data type (**homogeneous**) or can it hold elements of different types (**heterogeneous**).
* **Mutability**: Can the structure be changed after creation? Can elements be added, removed, or modified (**mutable**)? Or is the structure fixed once created (**immutable**)?
* **Iterability**: Is it possible in AlgoDraft to natively visit or process each element contained within the structure (e.g., using a `FOR EACH` loop)?
* **Duplicability**: Indicates whether elements (or certain parts, like keys or field names) are duplicate within the structure or not. This affects membership tests and structural integrity.

# **AlgoDraft Data Structures Characteristics/Categorizations**

| Structure   | Elements Topology | Homogeneous | Mutable | Duplicable | Iterable |
| ----------- | ----------------- | ----------- | ------- | ---------- | -------- |
| **Tuple**   | Linear            | No          | No      | Yes        | Yes¹     |
| **List**    | Linear            | No          | Yes     | Yes        | Yes¹     |
| **String**  | Linear            | Yes         | No      | Yes        | Yes¹     |
| **Set**     | Unstructured      | No          | Yes     | No         | Yes²     |
| **Mapping** | Associative       | No          | Yes     | No         | Yes³     |
| **Stack**   | Linear            | No          | Yes     | Yes        | Yes⁴     |
| **Queue**   | Linear            | No          | Yes     | Yes        | Yes⁵     |
| **Record**  | Unstructured      | No          | Yes     | No         | No⁶      |
| **Tree**    | Hierarchical      | No          | Yes     | Yes        | No⁷      |
| **Graph**   | Networked         | No          | Yes     | Yes        | No⁷      |
**Footnotes**

- ¹ **Tuple**, **List**, and **String** are iterable by **index-based traversal**.
    
- ² **Set** supports iteration in an **unspecified internal order**.
    
- ³ **Mapping** (associative arrays) can be iterated over **keys, values, or key-value pairs**, but without a guaranteed order.
    
- ⁴ **Stack** is iterable in **Last-In-First-Out (LIFO)** order.
    
- ⁵ **Queue** is iterable in **First-In-First-Out (FIFO)** order.
    
- ⁶ **Record** is not iterable — fields are accessed by name, not traversed.
    
- ⁷ **Tree** and **Graph** require **explicit traversal algorithms** (e.g., DFS, BFS); AlgoDraft does not offer a default iteration strategy for them.

# **Why This Matters in AlgoDraft**

Understanding these categories helps you write better pseudocode:

*   **Iterability:** Determines if you can use constructs like `FOR EACH element IN structure`.
*   **Sequentiality/Linearity:** Influences how you think about accessing elements (e.g., by index `list[i]`, by position `stack.Pop()`, or by relationships `node.LeftChild`).
*   **Mutability:** Tells you whether you can modify the structure in place or if operations create new structures.
*   **Non-Iterable Structures (like Record):** Require different access patterns, typically using dot notation (`record.field`).

By choosing a data structure whose properties match your needs, you can express your algorithms more clearly and efficiently in AlgoDraft.