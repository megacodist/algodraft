---
status: Draft
---
In AlgoDraft, an **Iterator** is a fundamental concept that represents the capability to produce a series of values, one value at a time, upon request. It's a way to interact with data that might be generated on the fly or come from a collection, without needing to have the entire collection loaded or pre-computed at once.

Iterators are the means by which you receive individual values from sources like:

# Iterators, AlgoDraft Vending Machines

Think of an Iterator like a high-tech **vending machine** that can only dispense items one by one. You don't know how many items are inside, or where they are stored internally. All you can do is interact with the machine using two specific controls that tell you its state and give you the next item if one is available:

- **Sold Out Lamp (Check Availability):** This tells you if the machine can dispense another item right now.
    
- **Dispense Button (Get Item):** This action attempts to get the next item.

# Operations

**`EXHAUSTED` Operator**
This unary operator returns `TRUE` if there is no value to yield, FALSE otherwise.

**`NEXT` Operator**
This unary operator retrieves the next available value. If the iterator is **already exhausted**, it returns an **undefined value**, but **does not break** the algorithm. This means the result of such a call is unpredictable and should not be relied upon in your pseudocode logic.

# Iterator States: Ready and Exhausted

An AlgoDraft Iterator exists in one of two distinct conceptual states, defined by its capacity to yield further values:

1. **The Ready State:** The iterator is in a state where it **is capable of yielding at least one more value**.
	
    - `EXHAUSTED $iter` returns `FALSE`.
    
    - Calling `NEXT $iter` is **valid**. It will cause the iterator to yield a value, and the iterator will then transition to either remain in the **Ready** state (if more values can be yielded) or move to the **Exhausted** state (if that was the last value).

2. **The Exhausted State:** The iterator is in a state where it **has already yielded all its values** and cannot yield any more.
	 
    - `EXHAUSTED $iter` returns `TRUE`.
    
    - Calling `NEXT $iter` is **invalid** in this state. It will result in an **undefined value**, and the iterator remains in the **Exhausted** state.

# Annotation and Instantiation

An Iterator in AlgoDraft is typically represented with a type annotation like `Iterator<T>`, where `T` indicates the type of value it will yield. While not strictly mandatory in all pseudocode contexts, using generics (`<T>`) is highly recommended for clarity, especially when the yielded values are consistently of one or a limited set of types.

You obtain an Iterator instance from sources capable of producing sequences:

1. **Streams:** Definitions for sequences of values generated on demand (defined using the `STREAM` keyword).
    
2. **Iterable Data Structures:** Data collections (like Lists, Stacks, etc.) designed to provide their contents one item at a time.

## Iterators from Streams

Calling a `STREAM` function initiates the process and provides an iterator that manages the stream's yielding logic.

```
STREAM SimpleCounter YIELDS Integer
    PARAMS
	    $limit AS Integer
    ENDPARAMS
    
    $i AS Integer <- 1
    WHILE $i <= $limit DO
        YIELD $i
        $i <- $i + 1
    ENDWHILE
    // Stream finishes and becomes exhausted when loop ends
ENDSTREAM


// Calling SimpleCounter(5) creates an Iterator<Integer> in the Ready state
$counterIter AS Iterator<Integer> <- SimpleCounter(5)
```

## Iterators from an Iterables

Applying the `ITERATOR OF` operator to an Iterable data structure requests an iterator that can yield its elements one by one.

```
$mylist AS List<String> <- ["apple", "banana", "cherry"]

// Applying ITERATOr OF to the list creates an Iterator<String> in the Ready state...
$lsIter AS Iterator<String> <- ITERATOR OF $mylist
```

# Example 1

Once you have an iterator, you can use the `EXHAUSTED` and `NEXT` operators to retrieve values safely, managing the iterator's state transitions. The standard, safe, and recommended pattern for consuming all values from an iterator is to repeatedly check if it's in the **Ready** state before attempting to get the next value.

```
$counterIter AS Iterator<Integer> <- SimpleCounter(3) // Using the stream from above
$currentValue AS Integer // Variable to hold the value from NEXT

NOTIFY {{starting to get values from the counter stream}}

// Loop as long as the iterator is in the Ready state (NOT Exhausted)
WHILE NOT (EXHAUSTED $counterIter) DO
    // Since EXHAUSTED is FALSE, we know calling NEXT is valid
    $currentValue <- NEXT $counterIter

    // Processing the value that was just yielded...
    NOTIFY {{next value is $current_value}}

    // The loop condition will check EXHAUSTED again before the next potential iteration
ENDWHILE

NOTIFY {{Getting values finished. The iterator is now in the Exhausted state.}}


// Expected notifications:
// starting to get values from the counter stream
// next value is 1
// next value is 2
// next value is 3
// Getting values finished. The iterator is now in the Exhausted state.
```

**Explanation:**

1. The `WHILE` loop condition `NOT (EXHAUSTED $counter_iter) `checks if the iterator is in the **Ready** state.

2. If it is **Ready** (`EXHAUSTED` is `FALSE`), the loop body executes.

3. Inside the loop, `NEXT $counterIter` is called. This is valid in the **Ready** state. The iterator yields a value, which is assigned to $currentValue, and its internal state updates (potentially moving towards **Exhausted**).

4. We then process $currentValue, which is guaranteed to be a valid yielded value because the check passed.

5. The loop repeats, checking the current state (`EXHAUSTED`) again before the next potential call to `NEXT`.

6. When the iterator has yielded its last value and transitioned to the **Exhausted** state, the next check `NOT (EXHAUSTED $counterIter)` will evaluate to `NOT TRUE`, which is `FALSE`, and the `WHILE` loop will terminate.

