---
status: Draft
---
In AlgoDraft, an **Iterator** is a fundamental built-in basic type. It represents the active mechanism or capability to produce a series of values, one value at a time, upon request. It's how you interact with data that might be generated on the fly (from an Stream Function) or come from a collection (via an `IIterable` object), without needing the entire sequence of values to be loaded or pre-computed at once.

Think of an Iterator as AlgoDraft's version of a high-tech **vending machine**. This machine can only dispense items one by one. You, as the user of the iterator, don't necessarily know how many items are inside, or how they are stored or generated internally. Your interaction is limited to two primary controls that inform you about the machine's state and allow you to get the next available item:

1. **"Sold Out" Lamp (Check Availability):** This tells you if the iterator can yield another value right now.

2. **"Dispense" Button (Get Item):** This action attempts to retrieve the next value from the iterator.

# Operations

- **`iterVar EXHAUSTED` Operator:**
    
    - **Purpose:** This unary postfix operator checks the state of the iterator to see if it can produce more values.
    
    - **Syntax:** `iterVar EXHAUSTED`
    
    - **Semantics:** Returns `TRUE` if the iterator has no more values to yield (it's "sold out"). Returns `FALSE` if there is at least one more value available.
	
    - **Negation**: Because it returns a Boolean value, it is a Boolean expression. There are two forms of negation:
	    - `NOT(iterVar EXHAUSTED)`
	    - `iterVar NOT EXHAUSTED`

- **`NEXT iterVar` Operator:**
    
    - **Purpose:** This unary prefix operator retrieves the next available value from the iterator.
    
    - **Syntax:** `valueVariable <- NEXT iterVar`
    
    - **Semantics:**
        
        - If the iterator is not exhausted (i.e., `iterVar EXHAUSTED` is `FALSE`), this operator retrieves the next value from the stream and the iterator prepares for the subsequent `NEXT` call.
        
        - If the iterator is **already exhausted** (i.e., `iterVar EXHAUSTED` is `TRUE`), calling `NEXT` returns an **undefined value**. Crucially, this **does not break** or halt the algorithm execution by itself, but the value obtained is unpredictable and should not be used. Always check `EXHAUSTED` before calling `NEXT`.

# Iterator States: Ready and Exhausted

Conceptually, an AlgoDraft Iterator is always in one of two distinct states, determined by its ability to yield further values:

1. **The Ready State:**
	
    The iterator is capable of yielding at least one more value.
    
    - `iterVar EXHAUSTED` returns `FALSE`.
    
    - Calling `NEXT iterVar` is **valid**. It will:
        
        1. Yield a value.
        
        2. The iterator will then either remain in the **Ready** state (if more values can be yielded) or transition to the **Exhausted** state (if that was the last value).

2. **The Exhausted State:**
	
    The iterator has already yielded all its values and cannot produce any more.
    
    - `iterVar EXHAUSTED` returns `TRUE`.
        
    - Calling `NEXT iterVar` in this state is **invalid** from a logical perspective. It will result in an **undefined value** being returned, and the iterator will remain in the **Exhausted** state.

# Type Annotation and Obtaining Iterator Instances

An Iterator in AlgoDraft is typically annotated with the type of value it yields, using generic notation: `Iterator<T>`, where `T` is the data type of the elements. For example, `Iterator<Integer>` yields integers, `Iterator<String>` yields strings. If an iterator can yield values of multiple types, `Iterator<ANY>` can be used.

You primarily obtain Iterator instances in AlgoDraft in two ways:

1. **From Stream Functions:** Calling a stream function (which uses `YIELD` to produce values) directly returns an `Iterator` object, ready to be consumed.
    
    ```
    STREAM SimpleCounter(limit AS Integer) YIELDS Integer :=
        i AS Integer <- 1
        WHILE i <= limit DO
            YIELD i
            i <- i + 1
        ENDWHILE
        // Iterator becomes exhausted when loop ends
    ENDSTREAM
    
    // Calling SimpleCounter(5) creates and returns an Iterator<Integer> in the Ready state
    counterIter AS Iterator<Integer> <- SimpleCounter(5)
    ```

2. **From Iterable Objects:** Applying the `ITERATOR OF` operator to an object that implements the `IIterable` interface (like `List`, `Tuple`, `String`) requests an iterator for that object's sequence.
    
    ```
    myNames AS List<String> <- ["apple", "banana", "cherry"]
    
    // Applying ITERATOR OF to the list creates an Iterator<String> in the Ready state
    listIterator AS Iterator<String> <- ITERATOR OF myNames
    ```

# Safe Consumption Pattern: Using `EXHAUSTED` and `NEXT`

The standard, safe, and recommended pattern for consuming all values from an iterator is to repeatedly check its state with `EXHAUSTED` before attempting to get the next value with `NEXT`.

**Example**:

```
// Using the SimpleCounter iteration function from above
sourceIterator AS Iterator<Integer> <- SimpleCounter(3)
currentValue AS Integer // Variable to hold the value from NEXT

NOTIFY {{starting to get values from the @sourceIterator}}

// Loop as long as the iterator is in the Ready state (i.e., NOT EXHAUSTED)
WHILE NOT (sourceIterator EXHAUSTED) DO
    // Since (EXHAUSTED sourceIterator) is FALSE, we know calling NEXT is valid
    currentValue <- NEXT sourceIterator

    // Processing the value that was just yielded...
    NOTIFY {{next value is @currentValue}}

    // The loop condition will check EXHAUSTED again before the next potential iteration
ENDWHILE

NOTIFY {{getting values finished. iterator @sourceIterator is now in the exhausted state.}}

// Expected notifications (actual output format depends on NOTIFY implementation):
// User informed: starting to get values from the sourceIterator
// User informed: next value is 1
// User informed: next value is 2
// User informed: next value is 3
// User informed: getting values finished. iterator sourceIterator is now in the exhausted state.
```

**Explanation of the Safe Consumption Pattern:**

1. The `WHILE` loop condition `NOT (sourceIterator EXHAUSTED)` checks if the iterator is in the **Ready** state.

2. If it is **Ready** (`EXHAUSTED` evaluates to `FALSE`, so `NOT (FALSE)` is `TRUE`), the loop body executes.

3. Inside the loop, `currentValue <- NEXT sourceIterator` is called. This is guaranteed to be a valid operation because the `EXHAUSTED` check passed. The iterator yields a value (which is assigned to `currentValue`), and its internal state updates, potentially moving it closer to or into the **Exhausted** state.

4. The yielded `currentValue` is then processed.

5. The loop repeats, and the state of `sourceIterator` (via `EXHAUSTED`) is checked again before another call to `NEXT` is attempted.

6. Once the iterator has yielded its last value and transitioned to the **Exhausted** state, the next evaluation of `NOT (sourceIterator EXHAUSTED)` will be `NOT (TRUE)`, which is `FALSE`. The `WHILE` loop then terminates.

This pattern ensures that `NEXT` is only called when the iterator is ready to provide a value, preventing reliance on undefined values from an exhausted iterator. While the FOR EACH loop (see Section [Cross-reference to FOR EACH]) often provides a more convenient way to iterate, understanding the underlying Iterator mechanics with EXHAUSTED and NEXT is crucial for more advanced scenarios or when implementing custom iteration logic.





**Explanation:**

1. The `WHILE` loop condition `NOT (EXHAUSTED $counter_iter) `checks if the iterator is in the **Ready** state.

2. If it is **Ready** (`EXHAUSTED` is `FALSE`), the loop body executes.

3. Inside the loop, `NEXT $counterIter` is called. This is valid in the **Ready** state. The iterator yields a value, which is assigned to $currentValue, and its internal state updates (potentially moving towards **Exhausted**).

4. We then process $currentValue, which is guaranteed to be a valid yielded value because the check passed.

5. The loop repeats, checking the current state (`EXHAUSTED`) again before the next potential call to `NEXT`.

6. When the iterator has yielded its last value and transitioned to the **Exhausted** state, the next check `NOT (EXHAUSTED $counterIter)` will evaluate to `NOT TRUE`, which is `FALSE`, and the `WHILE` loop will terminate.

