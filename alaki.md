---
status: Draft
---
This is a great idea! Introducing a `SortedList` is a natural and powerful addition to a data structures library. Let's break down the concepts and refine the API for it based on your notes.

**1. `SortedList` as a Value-Ordered Sequence**

*   **Your Point:** It's a "value-ordered sequence."
*   **Insight:** This is the perfect way to describe it.
    *   It's a **sequence**, meaning it's linear and ordered.
    *   The order is determined by **value** (using `IComparable`), not by insertion time or index (like a `List`).
    *   It must be **homogeneous** (`<T>`) because comparing heterogeneous types for ordering (`IComparable`) is often ill-defined or an error. The type `T` must implement `IComparable`.

*   **Interface Inheritance:**
    *   Your idea: `CLASS SortedList<T> INHERITS IROSequentiable<T>`
        *   What is `IROSequentiable`? Let's assume it's one of our previously discussed interfaces. For a `SortedList`, users would expect to be able to iterate it, get its length, check for membership, and probably read from it by index. So, it should implement at least **`IImmutableIndexable`**.
        *   Since it's sorted, it can't be `IMutableIndexable` (you can't `SET list[i] TO value` because that might break the sort order).
        *   Is it sliceable? Yes, slicing a `SortedList` to get another `SortedList` makes sense. So it should implement `ISliceable`.
        *   **Refined Interface:** `CLASS SortedList<T> IMPLEMENTS ISliceable` (which implies `IImmutableIndexable`, `IContainer`, `IIterable`) is a good fit.

**2. Instantiation**

*   **Your Proposal:** `METHOD NEW(iterable AS IIterable<T>)`
*   **Analysis:** This is a good constructor. It takes an existing iterable, sorts its elements, and creates the `SortedList`.
*   **Additional Constructors needed:**
    *   `METHOD NEW()`: To create an empty `SortedList`.
    *   `METHOD NEW(comparisonLogic AS ...)`: An advanced constructor to provide a *custom* sorting logic if the default `IComparable` implementation of `T` is not desired. This is advanced but very powerful. Let's stick to the simpler ones for now.
*   **Refined Instantiation:**
    ```pseudocode
    // Create an empty sorted list
    sl1 AS SortedList<Integer> <- NEW SortedList()

    // Create from an existing iterable (e.g., a List)
    unsorted AS List<Integer> <- [5, 1, 99, 10]
    sl2 AS SortedList<Integer> <- NEW SortedList(unsorted) // sl2 would conceptually be [1, 5, 10, 99]
    ```

**3. Merging an Iterable into a SortedList**

*   **Your Proposal:** `OPERATOR MERGE (iterable AS IIterable<T>) INTO this`
*   **Analysis:**
    *   **Keyword:** `MERGE` is the perfect keyword, as it implies an intelligent combination that preserves the sort order rule of the `SortedList`.
    *   **Operator vs. Method:**
        *   `OPERATOR MERGE iterable INTO this`: This implies an **in-place mutation**. `MERGE otherList INTO mySortedList`.
        *   If we follow the rule we established earlier that `MERGE` is an *expression that returns a new collection*, the syntax would be:
            `newSortedList <- MERGE iterable INTO mySortedList`
*   **Recommendation:** Let's stick to the consistent pattern.
    *   **`newSortedList <- MERGE iterable INTO oldSortedList`**: The expression returns a new, merged `SortedList`. The original is unchanged.
    *   **`mySortedList.MergeWith(iterable)`**: A method for in-place mutation.
    *   For the tutorial, let's focus on the non-mutating `MERGE` expression first.

**4. Getting the Insertion Index of a Value**

*   **Your Proposal:** A method to find the correct index where a new value *would be inserted* to maintain sort order.
*   **This is a critical and highly useful feature** for sorted sequences, often used as the basis for efficient insertion and for `bisect`-style algorithms.
*   **Your Distinction:** "before possibly equal values" vs. "after possibly equal values."
    *   This is the standard distinction made in binary search algorithms on sorted lists with duplicates.
    *   `FindInsertionPoint_BeforeEquals`: Often called `bisect_left` in Python. Finds the first index `i` where all `v < a[i]` are `True`.
    *   `FindInsertionPoint_AfterEquals`: Often called `bisect_right` in Python. Finds the first index `i` where all `v <= a[i]` are `True`.
*   **Syntax Suggestion (as methods):**
    Let's make this explicit with method names or parameters.
    *   **Option A (Two distinct methods):**
        *   `METHOD FindFirstInsertionIndex(IN value AS T) -> Integer`
        *   `METHOD FindLastInsertionIndex(IN value AS T) -> Integer`
    *   **Option B (One method with a flag):**
        *   `METHOD FindInsertionIndex(IN value AS T, IN position AS ("BEFORE" OR "AFTER") <- "BEFORE") -> Integer`
        *   (Using an `ENUM` or `String` for the flag).
*   **Recommendation:** Two distinct methods (`FindFirst...`, `FindLast...`) are often clearer and less error-prone than a flag.

---

**Refined Outline for `SortedList<T>` Tutorial**

**X.Z. `SortedList<T>`: A Value-Ordered Sequence**

   **X.Z.1. Introduction to `SortedList`**
   *   Define `SortedList<T>` as a data structure that maintains its elements in a **sorted order** at all times.
   *   **Key Characteristics:**
       *   **Value-Ordered:** The position of elements is determined by their value, not by insertion order.
       *   **Homogeneous:** It is a generic type (`<T>`). The element type `T` **must implement `IComparable`** to allow for ordering.
       *   **Dynamic:** Like a `List`, it can grow in size.
       *   **Efficient Lookups:** The sorted nature allows for very fast lookups, membership tests, and finding insertion points (typically `O(log n)`).
       *   **Slower Insertions:** Adding elements is slower than a regular `List` (`O(n)` in the worst case, because elements may need to be shifted), but `MERGE` operations can be efficient (`O(n+m)`).

   **X.Z.2. `SortedList<T>` and its Interfaces**
   *   State which interfaces `SortedList<T>` implements:
       *   `IMPLEMENTS ISliceable`: It can be read by index (`[]`) and sliced to produce a *new* `SortedList`.
       *   `IMPLEMENTS IContainer`: It has a `LENGTH OF`, supports `IN` (which can be very fast - `O(log n)`), and can be iterated (via `IIterable`).
       *   `IComparable` could be implemented for `SortedList` itself (to compare two sorted lists lexicographically), but this is a secondary feature.

   **X.Z.3. Instantiation**
   *   **a. Creating an Empty `SortedList`:**
       *   Syntax: `mySortedList AS SortedList<T> <- NEW SortedList()`
   *   **b. Creating from an Existing Iterable:**
       *   Syntax: `mySortedList AS SortedList<T> <- NEW SortedList(sourceIterable)`
       *   Semantics: The constructor takes any `IIterable<T>` (where `T` is `IComparable`), copies its elements, sorts them, and creates a new `SortedList`.
       *   Example:
           ```pseudocode
           unsortedNumbers AS List<Integer> <- [20, 5, 100, 1]
           sortedNumbers AS SortedList<Integer> <- NEW SortedList(unsortedNumbers)
           NOTIFY {{The sorted list is @sortedNumbers}} // User informed list is [1, 5, 20, 100]
           ```

   **X.Z.4. Combining and Adding Elements**
   *   **a. Merging an Iterable (`MERGE` expression):**
       *   Explain that `MERGE` is the primary way to combine `SortedList`s or add multiple items while preserving order.
       *   Syntax: `newSortedList <- MERGE iterable INTO originalSortedList`
       *   Semantics: This is a **non-mutating** operation that produces a **new** `SortedList`. It efficiently merges the elements from `iterable` into `originalSortedList`, maintaining the sort order. The `iterable`'s elements must also be of type `T`.
       *   Example:
           ```pseudocode
           listA AS SortedList<Integer> <- NEW SortedList([10, 30, 50])
           listB AS List<Integer> <- [25, 60, 10] // Contains a duplicate '10' for this example
           mergedList AS SortedList<Integer> <- MERGE listB INTO listA
           NOTIFY {{The merged sorted list is @mergedList}}
           // Result depends on whether SortedList allows duplicates. If not, it would be [10, 25, 30, 50, 60].
           // If it does, it could be [10, 10, 25, 30, 50, 60]. This needs to be specified.
           ```
   *   **b. In-place Insertion (Method):**
       *   For mutating the list, a dedicated method is used.
       *   Syntax: `mySortedList.Insert(element AS T)` (or `Add`)
       *   Semantics: Finds the correct position for `element` and inserts it in-place, shifting other elements as needed.
       *   Example:
           ```pseudocode
           myList AS SortedList<Integer> <- NEW SortedList([10, 30])
           myList.Insert(20) // myList is now [10, 20, 30]
           ```

   **X.Z.5. Finding Insertion Indices**
   *   Explain that `SortedList` provides methods to efficiently find the index where an element *would be* inserted, without actually inserting it. This is useful for many algorithms.
   *   Introduce the two variants to handle duplicates.
   *   **a. Finding the First Possible Position (`FindFirstInsertionIndex`):**
       *   Syntax: `index <- mySortedList.FindFirstInsertionIndex(value AS T)`
       *   Semantics: Returns the index of the first element in the list that is *greater than or equal to* `value`. This is the correct position to insert `value` to place it *before* any existing identical values.
   *   **b. Finding the Last Possible Position (`FindLastInsertionIndex`):**
       *   Syntax: `index <- mySortedList.FindLastInsertionIndex(value AS T)`
       *   Semantics: Returns the index of the first element in the list that is *strictly greater than* `value`. This is the correct position to insert `value` to place it *after* any existing identical values.
   *   **Example:**
       ```pseudocode
       // Assume SortedList allows duplicates for this example
       grades AS SortedList<Integer> <- NEW SortedList([70, 85, 85, 85, 90])
       // Indices:                                      0,  1,  2,  3,  4

       // Where would a new '85' go to be at the start of the '85' group?
       idxBefore AS Integer <- grades.FindFirstInsertionIndex(85)
       NOTIFY {{To insert 85 before others, use index @idxBefore}} // User informed: index 1

       // Where would a new '85' go to be at the end of the '85' group?
       idxAfter AS Integer <- grades.FindLastInsertionIndex(85)
       NOTIFY {{To insert 85 after others, use index @idxAfter}} // User informed: index 4

       // Where would '88' go?
       idx88 AS Integer <- grades.FindFirstInsertionIndex(88)
       NOTIFY {{To insert 88, use index @idx88}} // User informed: index 4
       ```

   **X.Z.6. Reading and Iterating**
   *   Briefly state that since `SortedList` implements `IImmutableIndexable`, all standard read operations are available: `mySortedList[index]`, `LENGTH OF mySortedList`, `element IN mySortedList` (which is very fast), `FOR EACH`, and slicing.
   *   Provide a quick example.

This outline provides a solid structure for introducing `SortedList`, covering its core concepts, instantiation, the crucial `MERGE` operator, and the very useful insertion index methods.