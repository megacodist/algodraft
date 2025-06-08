---
status: Draft
---
In AlgoDraft, many data structures and constructs are designed to represent sequences or collections of items. For these to be processed element by element, they must provide a mechanism to do so. The **`IIterable` interface** is the standard contract in AlgoDraft that defines this capability.

An object whose type `IMPLEMENTS IIterable` is called an "iterable." This means it can produce an **Iterator** – the "vending machine" that actually steps through and dispenses the items one at a time. Iterables themselves are often collections (like Lists or Sets) or specifications for generating sequences (like Intervals or Counts), and they may have other responsibilities beyond just producing an iterator.

**Syntax**:

The `IIterable` interface is very focused, defining a single essential operation:

```
INTERFACE IIterable :=
    // Operator to obtain an Iterator for this object's sequence of elements.
    OPERATOR ITERATOR OF this -> Iterator ENDOPERATOR
    // The 'Iterator' return type refers to AlgoDraft's basic Iterator type.
    // If the iterable has a known element type (e.g., List<String>),
    // the returned Iterator would conceptually be an Iterator<String>.
ENDINTERFACE
```

# Obtaining an Iterator: The ITERATOR OF Operator

The primary way to interact with an iterable object to begin iteration is by using its `ITERATOR OF` operator.

- **Purpose:** This unary prefix operator, when applied to an object that implements `IIterable`, requests and returns a new `Iterator` instance specifically bound to that object's sequence of data. Each call to `ITERATOR OF` on an iterable typically provides a fresh iterator, allowing for multiple independent traversals of the same iterable data if needed (though the specific behavior can depend on the iterable type).

- **Syntax:** `iteratorVar <- ITERATOR OF iterableVar`

- **Semantics:**
    
    - `iterableVar` must be an instance of a type that `IMPLEMENTS IIterable`.
    
    - The operation returns an Iterator object, initialized to be in the **Ready** state (positioned before the first element of the sequence from `iterableVar`).

**Example**:

```
myStringList AS List<String> <- ["alpha", "beta", "gamma"]
idSet AS Set<Integer> <- {101, 202, 303}

// Obtaining iterators explicitly using the ITERATOR OF operator:
stringListIter AS Iterator<String> <- ITERATOR OF myStringList
integerSetIter AS Iterator<Integer> <- ITERATOR OF idSet

// --- Manual iteration using the obtained list iterator ---
NOTIFY {{manual iteration through the list of strings is starting}}
currentItem AS String // Variable to hold the value from NEXT
WHILE NOT (stringListIter EXHAUSTED) DO
    currentItem <- NEXT stringListIter
    NOTIFY {{the current list item is @currentItem}}
ENDWHILE
NOTIFY {{manual iteration through the list of strings is complete}}
// Expected notifications (simplified):
// User informed: manual iteration through the list of strings is starting
// User informed: the current list item is alpha
// User informed: the current list item is beta
// User informed: the current list item is gamma
// User informed: manual iteration through the list of strings is complete

// --- Manual iteration using the obtained set iterator ---
NOTIFY {{manual iteration through the set of integers is starting}}
currentId AS Integer
WHILE integerSetIter NOT EXHAUSTED DO
    currentId <- NEXT integerSetIter
    NOTIFY {{the current set item is @currentId}}
ENDWHILE
NOTIFY {{manual iteration through the set of integers is complete}}
// Expected notifications (order for Set may not be guaranteed unless specified by Set type):
// User informed: manual iteration through the set of integers is starting
// User informed: the current set item is 101 (or other, depending on Set iteration order)
// User informed: the current set item is 202
// User informed: the current set item is 303
// User informed: manual iteration through the set of integers is complete
```

# Why Use ITERATOR OF Explicitly?

While high-level constructs like the `FOR EACH` loop implicitly handle the process of obtaining and using an iterator, there are scenarios where direct access to the iterator via `ITERATOR OF` is beneficial:

- **Granular Control:** For algorithms that need to manually advance an iterator, perhaps skipping elements or processing them in a non-standard way.

- **Passing Iteration State:** An Iterator object encapsulates the current state of an iteration. This iterator can be passed to other functions, allowing them to resume iteration from where it left off.

- **Multiple Concurrent Iterations:** You can obtain multiple independent iterators from the same iterable object to traverse it multiple times in parallel or in an interleaved fashion (if the iterable's nature supports this).

- **Implementing Custom Iteration Logic:** When the logic required is more complex than what a standard `FOR EACH` loop provides.

# Contrast with `FOR EACH` (Implicit Iteration):

The `FOR EACH` loop simplifies iteration by managing the iterator automatically.

```Algodraft
NOTIFY {{implicit iteration using FOR EACH loop for @myStringList is starting}}
element AS String
FOR EACH element IN myStringList DO // FOR EACH calls ITERATOR OF myStringList internally
    NOTIFY {{the current item via FOR EACH is @element}}
ENDFOR
```

Behind the scenes, `FOR EACH` uses the `IIterable` contract (specifically `ITERATOR OF`) and the resulting Iterator (with its `EXHAUSTED` and `NEXT` operations) to achieve its iteration.

# Summary

The `IIterable` interface, with its `ITERATOR OF` operator, is central to AlgoDraft's iteration model. It defines a standard way for collections and other sequence-producing types to provide an Iterator. This Iterator then allows for the step-by-step traversal of elements, either explicitly under your control or implicitly when using constructs like the `FOR EACH` loop. Understanding `IIterable` is key to understanding how AlgoDraft handles sequential data processing.