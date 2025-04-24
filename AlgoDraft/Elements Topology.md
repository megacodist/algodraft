---
status: Draft
---
In AlgoDraft, **Elements Topology** describes the _organizational form_ of data within a structure. It answers the fundamental question:

> _“How are the elements arranged in relation to each other?”_

This categorization does not reflect implementation details — it’s about **how we conceptually navigate or relate to the data**.
# **Linear, the Sequence Relationship**
In a **linear** topology, elements are organized in a single, ordered sequence — one after another — like links in a chain. The _"blind men and an elephant"_ parable reminds us that a concept can be approached from multiple perspectives, each shedding light on a different aspect of the whole. Similarly, linear topology can be understood through several complementary angles:

1. **Chain-Like Form**  
    Elements in a linear topology are arranged in a straight sequence, like links in a chain. Each element follows the previous one, forming a defined progression.
    
2. **Index Lookup**  
    Every element occupies a unique position in the sequence and can be accessed by its index. This position is typically based on a counting system—either **zero-based** or **one-based**, depending on the convention.
    
3. **Next–Previous Neighboring**  
    Each element (except the first) has a _predecessor_, and each (except the last) has a _successor_. This bidirectional awareness defines the core structure of linearity.

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

# **Hierarchical, the Parent-Child Relationship**
In a **hierarchical** topology, elements are arranged in a **parent-child** tree structure. Each element (except the root) has exactly one parent and have one or more children (except for leaves).

Hierarchical topology represents **nested** or **tiered** relationships, where some elements are _above_ or _contain_ others. Just like in an organizational chart or a family tree, multiple perspectives reveal the nature of this structure:

1. **Tree-Like Form**  
    The entire structure resembles a tree, with a **root** at the top and **branches** extending downward. Each node may have zero or more _children_ but only one _parent_ (except the root, which has none).
    
2. **Ancestry and Descendancy**  
    Elements are related through _generational depth_: each element has ancestors (parents, grandparents, etc.) and descendants (children, grandchildren, etc.). This lineage defines how information or control can flow within the hierarchy.

3.  **Ancestry Lookup**
	To uniquely locate or access a specific element, you often need **its full ancestral path**. This is known as **Ancestry Lookup**. The element's name alone may not be sufficient—its **context** (the hierarchy of containers or parents it resides in) is essential for disambiguation and access.
    
4. **Nested Scope**  
    Each element can be thought of as existing **within the context** of its parent. This nesting often implies containment, ownership, or scoping—e.g., in programming structures, document outlines, or organizational systems.
    
5. **Levels and Depth**  
    Elements occupy **levels** based on their distance from the root. These levels convey structural depth and often imply seniority, specificity, or precedence.

**Examples in AlgoDraft:**
- `Tree` (binary trees, general trees)

**Key Ideas:**
- Recursive structure: each node may be a subtree
- Natural for representing nested relationships
- Traversed using strategies like DFS or BFS

# **Associative, Key-Value-Pair Relationship**
In an **associative** topology, each element is identified by a **label** or **key** rather than a position. The focus is on accessing data _by identity_ rather than by location.

Associative topology organizes elements based on **pairings**: each _key_ is associated with a corresponding _value_. This model is widely used to represent mappings, dictionaries, or lookup tables. We can approach it through several perspectives:

1. **Two-Group Association**  
    The set of elements is divided into two logical groups: **keys** and **values**. Each key in the **domain** group uniquely identifies an element in the **codomain** group. This structure is often visualized as arrows pointing from keys to values.
    
2. **Key Lookup**  
    Instead of accessing elements by position or hierarchy, associative structures rely on a **search-by-key** mechanism. Keys serve as _identifiers_ to retrieve the corresponding value, much like querying a phone book by name to get a number.
    
3. **No Implied Order or Nesting**  
    There is no requirement for keys or values to follow a particular order or hierarchy. Each key-value pair stands independently, emphasizing **association over structure**.
    
4. **Function Analogy**  
    In mathematical terms, associative mappings often mirror the behavior of a **function**, where each input (key) maps to exactly one output (value). This analogy helps clarify the unidirectional and unique nature of key relationships.

**Examples in AlgoDraft:**
- `Mapping` (keys → values)
- `Record` (labels → values)

**Key Ideas:**
- Elements are accessed using labels, not positions
- There's no intrinsic "order" to elements
- Supports fast lookup by key
# **Networked, the Arbitrary-Connectivity Relationship**
In **network topology**, elements are connected by some form of relationship, but this relationship does **not conform to the structure** of linear sequences, key–value associations, or parent–child hierarchies. Instead, the connections are **arbitrary** — any element may connect to any other — giving rise to **non-deterministic and flexible relationships** typical of networks, graphs, and mesh-like structures.

Networked topology generalizes structure even further, where **elements can connect freely** without conforming to linear, hierarchical, or associative rules. Let’s explore this topology from multiple angles:

1. **Graph-Like Form**  
    The structure resembles a **graph** (in the mathematical sense), where elements (nodes) are interconnected by edges. Connections are not limited to a single "next" or "parent"—each element may link to any number of others.
    
2. **Arbitrary Connectivity**  
    There are **no fixed constraints** on how many elements each node can connect to, or in what pattern. This flexibility enables modeling of complex relationships such as social networks, transport systems, and dependency graphs.
    
3. **Bidirectional and Multidirectional Links**  
    Unlike linear or hierarchical topologies, relationships in networks are often **bidirectional** or even **multidirectional**. One element may be simultaneously related to multiple others in various roles.
    
4. **Decentralized Structure**  
    There is **no central root, starting point, or organizing principle**. Navigation must rely on traversal strategies (e.g., depth-first or breadth-first search) rather than positional or hierarchical clues.
    
5. **Relationship-Based Discovery**  
    Finding elements is not about index or ancestry, but about **traversing connections**—who's linked to whom. This makes **connectivity** the primary driver for data access and algorithmic design.

**Examples in AlgoDraft:**
- `Graph`

**Key Ideas:**
- Nodes may have many incoming/outgoing edges
- No fixed root or hierarchy
- Suitable for modeling relationships, routes, or dependencies
# **Unstructured Topology**
In **unstructured topologies**, the elements **do not exhibit any consistent or definable relationship** in terms of position, hierarchy, linkage, or mapping. There is **no predictable pattern** by which elements are related, ordered, or accessed.

This does **not** mean the data is chaotic or invalid — rather, it means that **AlgoDraft makes no assumption** about how the elements are related or accessed. The structure exists **only in terms of its content**, not its form.

The **Undefined Relationship** implies that elements are **logically grouped**, but the grouping is **not defined by element-to-element relationships**. It’s the absence of a topology, rather than a form of it.

**Examples in AlgoDraft:**
- `Set`

**Key Ideas:**
- No sequence or indexing
- Order is irrelevant
- Operations focus on presence/absence or set relationships
# Summary Table

| **Topology** | **Primary Relation**          | **Examples**        | **Access Style**            |
| ------------ | ----------------------------- | ------------------- | --------------------------- |
| Linear       | Sequence (next-previous)      | List, Tuple, String | By index or iteration order |
| Associative  | Key/label-value mapping       | Mapping, Record     | By key or field name        |
| Hierarchical | Parent–child relationships    | Tree                | By traversal (DFS, BFS)     |
| Networked    | Arbitrary connections (graph) | Graph               | By edge/link navigation     |
| Unstructured | No inherent relationship      | Set                 | By membership, equality     |
# Determining Elements Topology in AlgoDraft

**Is there any logical relationship among elements?**

1. **Yes** — elements are organized by a recognizable relationship:
    
    - **Are elements arranged one after another with implicit order?** → **Linear**
        
    - **Is there a clear superior–subordinate (parent–child) hierarchy?** → **Hierarchical**
        
    - **Can elements be split into two groups with a correspondence from one group (keys) to the other (values)?** → **Associative**
        
    - **None of the above, but elements still have arbitrary connections to others?** → **Networked**
        
2. **No** — there’s **no consistent or classifiable relationship** among the elements → **Unstructured**