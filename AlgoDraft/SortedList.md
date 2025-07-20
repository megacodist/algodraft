---
status: Draft
---
A `SortedList<T>` in AlgoDraft is a powerful data structure that maintains a collection of elements in a **sorted order** at all times. Unlike a standard `List` where element order is determined by the sequence of insertions, a `SortedList`'s order is determined by the intrinsic value of the elements themselves.

**Key Characteristics:**

*   **Value-Ordered:** The position of any element within the list is based on a defined comparison rule, ensuring the collection remains sorted after every modification.

* **Mutable**: `SortedList`s are mutable, but modifications are controlled to preserve the sort order.

*   **Generic (`<T>`):** `SortedList<T>` is a generic type, meaning it can hold elements of any specific type `T`. By default, the element type `T` **must implement `IComparable`** to provide the necessary comparison logic for sorting.

*   **Dynamic:** The size of the list can grow or shrink as elements are added or removed.

*   **Efficient Lookups:** The sorted nature allows for very fast operations like membership testing (`IN`) and finding insertion points, which are typically performed in logarithmic time (`O(log n)`).

*   **Ordered Insertions:** Adding new elements is generally slower than appending to an unsorted `List` (as it's an `O(n)` operation in the worst case, because existing elements may need to be shifted to maintain order), but the collection's sorted state is always guaranteed.

The `SortedList<T>` class implements the `IROSequentiable<T>` interface. This is a crucial aspect of its design, as it means a `SortedList` provides a rich set of read-only and comparison capabilities:

*   It is a **Read-Only Sequence:**
    *   You can iterate through its elements in their sorted order using a `FOR EACH` loop (as it is an `IIterable`).
    *   You can get its size with `LENGTH OF` (as it is an `IContainer`).
    *   You can test for membership with `IN` (as it is an `IContainer`).
    *   You can access elements by their index using `mySortedList[index]` (as it is `IROIndexable`).
    *   You can create a *new* `SortedList<T>` by taking a slice of an existing one using `Interval` or `Count` objects (as it is `IROSequentiable`).
*   It is **Comparable:**
    *   Since `IROSequentiable<T>` inherits `IComparable`, two `SortedList<T>` instances can be compared to each other using operators like `<`, `>`, `=`, etc. This comparison is performed using lexicographical ("dictionary") order on their sorted elements.

# Operations
## Instantiation and Custom Sorting

A `SortedList` can be created empty or from an existing oterable. Crucially, you can also provide a custom sorting function if the default ordering of the element type is not what you need.

**a. Basic Constructors**
*   **`METHOD NEW()`**: Creates an empty `SortedList<T>`. The default `IComparable` implementation of the element type `T` will be used for sorting all added elements.
    ```algodraft
    // Creating an empty sorted list that will order integers numerically.
    scores AS SortedList<Integer> <- NEW SortedList()
    ```
*   **`METHOD NEW(iterable AS IIterable<T>)`**: Creates a `SortedList<T>` from an existing iterable collection. The elements from the source iterable are copied and sorted upon creation using `T`'s default `IComparable` behavior.
    ```algodraft
    unsortedData AS List<Integer> <- [20, 5, 100, 1, 5]
    // The new SortedList will contain [1, 5, 5, 20, 100] (assuming it allows duplicates).
    sortedData AS SortedList<Integer> <- NEW SortedList(unsortedData)
    ```

### Advanced Constructor with a Custom Sorter

For situations where you need a non-default sorting order, you can provide a custom comparison function to the `SortedList` constructor. The `sorter` parameter must be a function that matches the signature `Function((T, T) -> Integer)`. This function takes two elements of type `T` (`item1`, `item2`) and must return:

*   A **negative integer** (`< 0`) if `item1` should come *before* `item2`.
*   A **zero integer** (`0`) if `item1` and `item2` are considered *equal* for sorting purposes.
*   A **positive integer** (`> 0`) if `item1` should come *after* `item2`.

**Syntax:**

```
 METHOD NEW(sorter AS Function((T, T) -> Integer) OR NULL <- NULL)
 METHOD NEW(iterable AS IIterable<T>, sorter AS Function((T, T) -> Integer) OR NULL <- NULL)
```

**Example:**

```algodraft
// A custom sorter function to sort strings by their length instead of alphabetically.
FUNCTION SortStringsByLength(IN str1 AS String, IN str2 AS String) -> Integer :=
	RETURN (LENGTH OF str1) - (LENGTH OF str2)
ENDFUNCTION

// Creating an empty SortedList that will use this custom sorter...
namesByLength AS SortedList<String> <- NEW SortedList(sorter <- SortStringsByLength)

// Populating it using its methods...
INSERT "Charles" INTO namesByLength
INSERT "Al" INTO namesByLength
INSERT "Barbara" INTO namesByLength
// The list now contains ["Al", "Charles", "Barbara"]

// Or creating from an existing list with the sorter
initialNames AS List<String> <- ["Eve", "David"]
finalNames AS SortedList<String> <- NEW SortedList(initialNames, sorter <- SortStringsByLength)
// The list now contains ["Eve", "David"]
```

## Inserting a Single Element

To add a single element to a SortedList while maintaining its sort order, you use a **Natural Language Instruction (NLI)**. This approach provides fine-grained control, especially when dealing with lists that may contain duplicate values. The NLI allows you to specify whether the new element should be placed before or after any existing elements of equal value.

**Syntax:**

```
DO {{Insert @value before/after possibly equal values into @slt.}}
```

The NLD must contain:

- The value, referenced as an `@value`.

- The target sorted list, referenced as an `@slt`.

- One of the words **before** or **after** to specify placement relative to existing equal values.

**Semantics:**

- The `SortedList` efficiently finds the correct position for value based on its defined sort order (either the default `IComparable` behavior of the element type or the custom sorter provided at instantiation).

- If before is specified, the new element is inserted at the earliest possible index where it maintains the sort order (i.e., before any other elements of the same value).

- If after is specified, the new element is inserted at the latest possible index where it maintains the sort order (i.e., after any other elements of the same value).

- The SortedList is modified in place, with existing elements being shifted as necessary to accommodate the new element.

**Example:**

```algodraft
grades AS SortedList<Integer> <- NEW SortedList([70, 85, 85, 90])
NOTIFY {{Initial list: @grades}} // User informed: [70, 85, 85, 90]

newGrade1 AS Integer <- 85
// Insert the new '85' at the start of the existing '85' block.
DO {{Insert @newGrade1 into @grades before equal values}}
NOTIFY {{After inserting 85 before: @grades}} // User informed: [70, 85, 85, 85, 90]

newGrade2 AS Integer <- 85
// Insert the new '85' at the end of the existing '85' block.
DO {{Insert @newGrade2 into @grades after equal values}}
NOTIFY {{After inserting 85 after: @grades}} // User informed: [70, 85, 85, 85, 85, 90]

newGrade3 AS Integer <- 65
// For values with no duplicates, 'before' and 'after' result in the same position.
DO {{Insert @newGrade3 into @grades before equal values}}
NOTIFY {{After inserting 65: @grades}} // User informed: [65, 70, 85, 85, 85, 85, 90]
```

## Merging an Iterable

The `MERGE` operator is the primary way to efficiently add all elements from an iterable collection into an existing `SortedList`.

**Syntax:**

```
OPERATOR MERGE (iterable AS IIterable<T>) INTO this
```

**Semantics:**

This is an **in-place mutating** operation. Each element from `iterable` is efficiently inserted into the sorted list at its correct sorted position. The `MERGE` operation is typically more efficient than inserting elements one by one in a loop.

**Example:**

```algodraft
mainScores AS SortedList<Integer> <- NEW SortedList([70, 90])
newScores AS List<Integer> <- [65, 90, 100] // May contain duplicates

// Merge the new scores into the main list.
MERGE newScores INTO mainScores
NOTIFY {{The list is now @mainScores}}
// User is informed the list is [65, 70, 90, 90, 100] (assuming SortedList allows duplicates).
```

## Finding Insertion Index (NLI)

A key feature of a `SortedList` is its ability to efficiently find the correct index for a new element without actually performing the insertion. This is achieved in AlgoDraft using a **Natural Language Instruction (NLI)**.

**Syntax:**

```
DO {{Find insertion index of @var before/after possibly equal values into @slt.}} -> Integer
```

The NLI must contain:

*   The value to be searched for, referenced as an `@var`.
*   One of the words `before` or `after` to specify how to handle cases where values equal to the search value already exist in the list.
* The sorted list itself (`@slt`).
*   The `DO` expression returns the `Integer` index where the value should be inserted to maintain the list's sort order.

**Semantics:**

Returns the suitable index at which the value must be inserted without inserting it.

**Example:**

```algodraft
// Assuming this SortedList allows duplicate values...
grades AS SortedList<Integer> <- NEW SortedList([70, 85, 85, 85, 90])
// Indices:                                      0,  1,  2,  3,  4

gradeToPlace AS Integer <- 85

// Finding the index to insert at the very beginning of the '85' block...
idxBefore AS Integer <- grades DO {{Find insertion index of @gradeToPlace before equal values}}
NOTIFY {{To insert @gradeToPlace before existing equals, use index @idxBefore}} // User informed: use index 1

// Find the index to insert at the very end of the '85' block.
idxAfter AS Integer <- grades DO {{Find insertion index of @gradeToPlace after equal values}}
NOTIFY {{To insert @gradeToPlace after existing equals, use index @idxAfter}} // User informed: use index 4

otherGrade AS Integer <- 88
idx88 AS Integer <- grades DO {{Find insertion index of @otherGrade before equal values}}
NOTIFY {{To insert @otherGrade, use index @idx88}} // User informed: use index 4
```

## Deleting at an Index

**Syntax**:

```
OPERATOR DEL this[idx AS Integer]
```

## Read-Only Sequence operations in `SortedList`

Since `SortedList<T>` implements `IROSequentiable<T>`, all standard read-only and comparison operations are available:

*   **Read by Index:** `value <- mySortedList[index]`
*   **Get Size:** `size <- LENGTH OF mySortedList`
*   **Membership Test:** `isPresent <- element IN mySortedList` (This is very fast, `O(log n)`).
*   **Iteration:** `FOR EACH element IN mySortedList DO ...` (Iterates in sorted order).
*   **Slicing:** `newSortedList <- mySortedList[interval_spec]` (Returns a new `SortedList`).
*   **Comparison:** `isLess <- sortedListA < sortedListB` (Performs a lexicographical comparison).