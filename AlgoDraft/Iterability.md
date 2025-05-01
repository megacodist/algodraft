---
status: Writing
---
In AlgoDraft, **iterability** refers to whether a data structure comes with a **guaranteed, built-in iteration mechanism** that allows traversal of its elements using a consistent and well-understood control construct such as a `FOR EACH` loop.

This property does **not** concern whether it is theoretically possible to traverse the elements — instead, it denotes whether AlgoDraft itself **endorses and enables such traversal inherently**.

* **Iterable**: AlgoDraft provides a built-in way to traverse.  
* **Non-Iterable**: You must define your own traversal logic.

# **Non-Iterable**

A **non-iterable** data structure in AlgoDraft is one for which **no standard iteration method is defined or guaranteed**. You cannot use `FOR EACH` directly on them without explicitly defining a traversal logic.

**Examples in AlgoDraft**:

- **Records**: Fields are accessed by name, not by traversal.

- **Trees and Graphs**: Traversal strategies (like DFS, BFS) **exist**, but must be explicitly invoked or implemented — AlgoDraft does not assume a single, inherent way to iterate over them.

> **Important**: Non-iterable does _not_ mean untraversable. You can still walk through the elements, but you must **impose your own logic** (e.g., define a DFS function for a tree).

# **Iterable**

An **iterable** data structure is one for which AlgoDraft **ensures a native, direct traversal strategy**
and expect it to work without having to invent your own traversal method. The structure defines or implies **how** to walk through its contents.

**Examples in AlgoDraft**:

- **Lists**: Iterated from first to last element.

- **Sets**: Iterated in an undefined, but guaranteed way.

- **Mappings**: Can be iterated over keys, values, or key-value pairs.

- **Stacks/Queues**: Support iteration in their operational order (LIFO/FIFO).

> **Note**: Some structures like **Sets** or **Mappings** do not define a meaningful _order_ for traversal, but **still count as iterable** because AlgoDraft supports traversing their contents natively.

## Operations
### ITERAOTR OF Operator, Obtaining an Iterator

While constructs like `FOR EACH` implicitly handle the process of stepping through an iterable's elements, sometimes you need more direct and granular control over the iteration mechanism. Iterable structures live up to their name: they can produce an [[Iterator Basic Type|iterator]]. An Iterator is essentially a conceptual object that knows how to traverse the associated stream of data one by one. The `ITERATOR OF` operator allows you to explicitly obtain an iterator from an iterable data structure.

This is a unary operator that, when applied to an iterable data structure (on the right of the operator), returns an [[Iterator Basic Type|iterator]] object specifically bound to that instance of the data structure.

```
$myList AS List<String> <- ["apple", "banana", "cherry",]
$mySet AS Set<Integer> <- {10, 20, 30,}

// Obtaining iterators explicitly...
$listIterator AS Iterator<String> <- ITERATOR OF $myList
$setIterator AS Iterator<Integer> <- ITERATOR OF $mySet

// Manual iteration using the list iterator
NOTIFY {{manually iterating through the list of strings is started}}
WHILE NOT EXHAUSTED $listIterator DO
    $currentItem AS String <- NEXT $listIterator
    NOTIFY {{the current item is $currentItem}}
ENDWHILE
NOTIFY {{manual iteration through the list of strings is completed}}

// Manual iteration using the set iterator
NOTIFY {{manually iterating through the set of integers is started}}
WHILE NOT EXHAUSTED $setIterator DO
    $currentItem AS Integer <- NEXT $setIterator
    NOTIFY {{the current item is $currentItem}}
ENDWHILE
NOTIFY {{manual iteration through the set of integers is completed}}

// Contrast with implicit iteration (which uses an iterator behind the scenes)
NOTIFY {{implicit iteration using in FOR EACH loop is started}}
FOR EACH $currentItem IN $myList DO
    NOTIFY {{the current item is $currentItem}}
ENDFOR
```

Using `ITERATOR OF` gives you fine-grained control, useful for algorithms that might need to manually advance an iterator, pass the iteration state around, or implement custom looping logic not easily captured by a standard `FOR EACH` loop.