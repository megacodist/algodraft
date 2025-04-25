---
status: Draft
---
**Mutability** refers to whether the contents of a data structure can be **changed after creation**. This includes modifying elements, adding new elements, or removing existing ones. In AlgoDraft, mutability is not about low-level memory operations but about **conceptual changeability**—whether a data structure is **mutable** or **immutable** by design.
# Mutable
A mutable structure **allows changes** to its contents. You can:

- Modify existing elements
    
- Insert new elements
    
- Remove existing elements
    

**Implications in AlgoDraft:**

- Algorithms can build up or tear down such structures during execution.
    
- Mutable structures are often used for **temporary state**, **accumulation**, or **dynamic modeling**.
    

**Examples:**

- Lists (you can append, remove, or change elements)
    
- Sets (you can add or discard members)
    
- Mappings (you can assign new key-value pairs, remove or change existing ones)
    
- Stacks and Queues (you can push or pop elements)
# **Immutable**

An immutable structure **cannot be changed** after its creation. Any “modification” yields a **new instance**. This does **not mean it's constant**, but that its **internal state is fixed** once defined.

**Implications in AlgoDraft:**

- Emphasizes **safety**, **predictability**, and **referential transparency**.
    
- Often used for **fixed configuration**, **pattern-matching**, or **data integrity**.
    

**Examples:**

- Strings (changing a character yields a new string)
    
- Tuples (you cannot add or remove items once formed)