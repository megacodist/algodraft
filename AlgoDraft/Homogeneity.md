---
status: Draft
---
**Homogeneity** refers to whether the elements within a data structure are of **the same type** or **different types**. It describes the **type uniformity** of the structure’s contents.

In AlgoDraft, homogeneity is a **semantic attribute**, not a restriction. Most structures are **heterogeneous by design** — they allow mixed-type elements — but a programmer may use them in a homogeneous way.

# Homogeneous
A data structure is **homogeneous** when **all of its elements are of the same type**. Very few data structures intrinsically require type uniformity and others can employ it for **clarity, safety, or algorithmic consistency** (a matter o usage).

If a data structure is a homogeneous, generics should be used for emphasis `$list <- CREATE List<Integer>`.

**Characteristics**
- Simplifies reasoning about operations (e.g., summing numbers).
- Preferred in mathematical and statistical algorithms.
- Common in Strings, which are character sequences.
# Heterogeneous
A data structure is **heterogeneous** when its elements **can differ in type**. This is AlgoDraft’s **default interpretation** for most container types (unless noted otherwise).

**Characteristics**:
- Enables flexible data modeling (like JSON, mappings, records).
- Useful for real-world entities with diverse properties.
- Makes structures like Mappings and Records powerful and expressive.