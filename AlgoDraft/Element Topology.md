---
status: Draft
---
In AlgoDraft, **Element Topology** describes the _organizational form_ of data within a structure. It answers the fundamental question:

> _“How are the elements arranged in relation to each other?”_

This categorization does not reflect implementation details — it’s about **how we conceptually navigate or relate to the data**.
# **Linear Topology**
In a **linear** topology, elements are organized in a single, ordered sequence — one after another — like links in a chain. Each element has a clear predecessor and successor (except the first and last).

**Examples in AlgoDraft:**
- `Tuple`
- `List`
- `String`
- `Stack` (conceptually linear with LIFO access)
- `Queue` (conceptually linear with FIFO access)

**Key Ideas:**
- Position matters (first, second, etc.)
- Often indexed or accessed by position
- Traversal usually left-to-right or front-to-back
# **Associative Topology**
In an **associative** topology, each element is identified by a **name** or **key** rather than a position. The focus is on accessing data _by identity_ rather than by location.

**Examples in AlgoDraft:**
- `Mapping` (keys → values)
- `Record` (field names → values)

**Key Ideas:**
- Elements are accessed using labels, not positions
- There's no intrinsic "order" to elements
- Supports fast lookup by key
# **Hierarchical Topology**
In a **hierarchical** topology, elements are arranged in a **parent-child** tree structure. Each element (except the root) has exactly one parent and may have multiple children.

**Examples in AlgoDraft:**
- `Tree` (binary trees, general trees)

**Key Ideas:**
- Recursive structure: each node may be a subtree
- Natural for representing nested relationships
- Traversed using strategies like DFS or BFS
# **Networked Topology**
A **networked** topology allows elements to connect in arbitrary ways — any node may link to any number of others, possibly including cycles. The relationships form a **graph** rather than a simple path or tree.

**Examples in AlgoDraft:**
- `Graph`

**Key Ideas:**
- Nodes may have many incoming/outgoing edges
- No fixed root or hierarchy
- Suitable for modeling relationships, routes, or dependencies
# **Unstructured Topology**
In an **unstructured** topology, elements exist as a collection with **no intrinsic order or connection** between them. They are simply grouped together without position, hierarchy, or identity beyond their own values.

**Examples in AlgoDraft:**
- `Set`

**Key Ideas:**
- No sequence or indexing
- Order is irrelevant
- Operations focus on presence/absence or set relationships
# Summary Table

|**Topology**|**Primary Relation**|**Examples**|**Access Style**|
|---|---|---|---|
|Linear|Position-based|List, Tuple, String|By index or iteration order|
|Associative|Key/name-based|Mapping, Record|By key or field name|
|Hierarchical|Parent–child relationships|Tree|By traversal (DFS, BFS)|
|Networked|Arbitrary connections (graph)|Graph|By edge/link navigation|
|Unstructured|No inherent relationship|Set|By membership, equality|
