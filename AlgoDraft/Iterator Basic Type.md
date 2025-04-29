---
status: Writing
---
In AlgoDraft, an **Iterator** is a fundamental concept that represents the capability to produce a series of values, one value at a time, upon request. It's a way to interact with data that might be generated on the fly or come from a collection, without needing to have the entire collection loaded or pre-computed at once.

Iterators are the means by which you receive individual values from sources like:

1. **Streams:** Definitions for sequences of values generated on demand (defined using the `STREAM` keyword).
    
2. **Iterable Data Structures:** Data collections (like Lists, Stacks, etc.) designed to provide their contents one item at a time.

# Iterators, AlgoDraft Vending Machines

Think of an Iterator like a high-tech **vending machine** that can only dispense items one by one. You don't know how many items are inside, or where they are stored internally. All you can do is interact with the machine using two specific controls that tell you its state and give you the next item if one is available:

- **Sold Out Lamp (Check Availability):** This tells you if the machine can dispense another item right now.
    
- **Dispense Button (Get Item):** This action attempts to get the next item.

In AlgoDraft, these controls correspond to the two primary operators on an Iterator.

# Operations

**`EXHAUSTED` Operator**
This unary operator returns `TRUE` if there is no value to yield, FALSE otherwise.

**`NEXT` Operator**
This unary operator retrieves the next available value. If the iterator is **already exhausted**, it returns an **undefined value**, but **does not break** the algorithm.

# Iterator States: Ready and Exhausted

An AlgoDraft Iterator exists in one of two distinct conceptual states, defined by its capacity to yield further values:

1. **The Ready State:** The iterator is in a state where it **is capable of yielding at least one more value**.
	
    - `EXHAUSTED $iter` returns `FALSE`.
    
    - Calling `NEXT $iter` is **valid**. It will cause the iterator to yield a value, and the iterator will then transition to either remain in the **Ready** state (if more values can be yielded) or move to the **Exhausted** state (if that was the last value).

2. **The Exhausted State:** The iterator is in a state where it **has already yielded all its values** and cannot yield any more.
	 
    - `EXHAUSTED $iter` returns `TRUE`.
    
    - Calling `NEXT $iter` is **invalid** in this state. It will result in an **undefined value**, and the iterator remains in the **Exhausted** state.

# Iterator Type and Creation

An Iterator in AlgoDraft is typically represented with a type annotation like `Iterator<T>`, where `T` indicates the type of value it will yield. While not strictly mandatory in all pseudocode contexts, using generics (`<T>`) is highly recommended for clarity, especially when the yielded values are consistently of one or a limited set of types.

You obtain an Iterator instance from sources capable of producing sequences:

## From a Stream

Calling a STREAM definition initiates the process and provides an iterator that manages the stream's yielding logic.

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
$counter_iter AS Iterator<Integer> <- SimpleCounter(5)
```