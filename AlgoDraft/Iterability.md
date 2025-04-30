---
status: Writing
---
**Iterability** refers to whether a data structure’s elements can be **visited one by one in a defined and repeatable manner**. In AlgoDraft, iterability doesn’t imply how the iteration is implemented, but **whether the concept of “visiting elements” makes sense** for the structure.

# **Non-Iterable**

A non-iterable structure **does not support** element-wise traversal because its elements **aren’t structured for stepping through**. These structures are **accessed directly by name or position**, and not through looping.

**Implications in AlgoDraft:**
- You cannot use `FOR EACH` directly.
- Processing typically involves **direct access by field or method**, not iteration.

**Examples:**
- **Records**: You don’t iterate over fields; you access them by name (e.g., `person.age`)
- Some abstract containers or wrappers without iterable semantics

# **Iterable**
An iterable structure allows traversal over its elements, typically using **loops**, **traversals**, or **other step-wise mechanisms**. The **order of iteration** may be:

- Well-defined (as in Lists or Strings),
- Algorithmically defined (as in Trees or Graphs),
- Undefined but still supported (as in Sets).

**Implications in AlgoDraft:**
- You can use iteration constructs like `FOR EACH` with them.
- Essential for algorithms that **examine**, **process**, or **filter** data.

**Examples:**
- Lists, Strings, Tuples (in their natural order)
- Sets (order not guaranteed)
- Mappings (can iterate over keys, values, or key-value pairs)
- Trees and Graphs (through DFS, BFS, etc.)
- Stacks and Queues (though they’re usually processed rather than walked)

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