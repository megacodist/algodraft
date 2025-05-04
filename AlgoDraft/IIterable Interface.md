---
status: Writing
---
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
