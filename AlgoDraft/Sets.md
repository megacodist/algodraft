---
status: Writing
---
Sets are an **unstructured, heterogeneous, mutable, iterable, and non-duplicable** data structure in AlgoDraft:

* **Unstructured**: Sets have no internal organization such as hierarchy or lineage. Elements exist in an unstructured "pool," with no relationship to each other beyond co-membership in the set.
	- No first, last, parent, or neighbor.
    - Membership is the key relation — _is this element in the set?_

* **Heterogeneous**: While practical usage often favors uniform types (e.g., `Set<Integer>`), this is not required by AlgoDraft’s design and they are allowed sets to contain elements of any type.

*  **Mutable**: Elements can be added, removed, and/or altered at any time.

*  **Iterable**: Although the order is not guaranteed, sets can be iterated over using constructs like `FOR EACH`, although the iteration order is unspecified.

* **Non-duplicable**: Duplicate elements are not allowed. If an equivalent element already exists in the set, insertion has no effect.

They are used to represent collections of **distinct** elements where **order does not matter** and **each element appears only once**.
# Instantiating
```
// Defining a set using literal notation...
$set1 AS Set<Integer> <- {1, 5, 2, 5} // Becomes {1, 2, 5}
$set2 AS Set<Integer OR String> <- {"apple", 3, "apple",} // Becomes {3, "apple",}
$set3 AS Set <- {1, "hello", TRUE, 1} // Becomes {1, "hello", TRUE}

// Defining an empty set...
$empty1 AS Set<SomeType> <- CREATE AN EMPTY Set<SomeType>
$empty2 AS Set<SomeType> <- CREATE Set<SomeType>
$empty3 AS Set<SomeType> <- ∅
```

**Notes**
* In set literals, trailing comma is allowed and it is a good practice to add values later easily.
* `AN EMPTY` although verbose, it is a good practice to emphasize the emptiness of the newly created set.
# Operations

## **Iteration**

In AlgoDraft, you can process each element within a set using the **`FOR EACH`** loop.  
This construct lets you perform actions for every item in the set.

> **Important:**  
> The order in which elements are processed is **not guaranteed**. Sets in AlgoDraft are inherently unordered collections.

**Syntax**

```
FOR EACH $element IN $set DO
   // operations using $element
ENDFOR
```

- `$element` is a **temporary variable** that represents the current element during each iteration.
    
- `$set` is the set you want to iterate through.
    
- The code inside the loop body (`DO ... ENDFOR`) runs once for each element in the set.

## Equality/Inequality Operators

These binary operators perform a check, returning a Boolean value, to determine if two sets are considered equal or unequal. Set equality in AlgoDraft, following mathematical convention, means that both sets contain exactly the same elements, irrespective of the order in which elements were added.

AlgoDraft provides multiple syntaxes for expressing equality and inequality:
* **Equality**:
	* `=` (Symbolic, mathematical standard)
* **Inequality**:
	* != (Symbolic, common programming standard)
	* `≠` (Symbolic, mathematical standard)

```
// Defining some sets for comparison using AlgoDraft syntax...
$set1 AS Set<Integer> <- {10, 20, 30}
$set2 AS Set<Integer> <- {30, 10, 20} // Same elements, different literal order
$set3 AS Set<Integer> <- {10, 20, 40} // Different elements
$set4 AS Set<Integer> <- {10, 20}    // Different elements (subset)

NOTIFY {{Comparing set1 = {10, 20, 30}, set2 = {30, 10, 20}, set3 = {10, 20, 40}, set4 = {10, 20}.}}

// --- Equality Checks ---
IF $set1 = $set2 THEN
   NOTIFY {{$set1 is equal to $set2 using the = operator.}} // Expected
ELSE
   NOTIFY {{$set1 is not equal to $set2 using the = operator.}}
ENDIF

IF $set1 = $set3 THEN
   NOTIFY {{$set1 is equal to $set3 using the = operator.}}
ELSE
   NOTIFY {{$set1 is not equal to $set3 using the = operator.}} // Expected
ENDIF

// --- Inequality Checks ---
IF $set1 != $set3 THEN
   NOTIFY {{$set1 is not equal to $set3 using the != operator.}} // Expected
ELSE
   NOTIFY {{$set1 is equal to $set3 using the != operator.}}
ENDIF

IF $set1 != $set2 THEN
   NOTIFY {{$set1 is not equal to $set2 using the != operator.}}
ELSE
   NOTIFY {{$set1 is equal to $set2 using the != operator.}} // Expected
ENDIF

IF $set1 ≠ $set3 THEN
   NOTIFY {{$set1 is not equal to $set3 using the ≠ symbol.}} // Expected
ELSE
   NOTIFY {{$set1 is equal to $set3 using the ≠ symbol.}}
ENDIF

IF $set1 ≠ $set2 THEN
   NOTIFY {{$set1 is not equal to $set2 using the ≠ symbol.}}
ELSE
   NOTIFY {{$set1 is equal to $set2 using the ≠ symbol.}} // Expected
ENDIF
```
## Membership/Containment Operators

Membership (containment) operators in AlgoDraft check whether an element is a member of a set. These operators are **binary**: they take two operands and return a **Boolean** result (`TRUE` or `FALSE`).

They are based on **mathematical semantics**, and are typically used inside conditional statements to control the flow of algorithms based on whether an element exists within a set.

|Operator|Meaning|
|---|---|
|`IN`, `∈`|The left operand _is_ a member of the right operand.|
|`NOT IN`, `∉`|The left operand _is not_ a member of the right operand.|

**Membership Semantics**

- The **left operand** must be a single element.

- The **right operand** must be a `Set`.

- Membership checking is based on **element equality**.

* Both symbolic (`∈`, `∉`) and text-based (`IN`, `NOT IN`) forms are supported, and they behave identically.

**Example**
```
$numbers AS Set<Integer> <- {10, 20, 30}

IF 30 IN $numbers THEN
   NOTIFY {{30 is found}}
ENDIF

IF 20 ∈ $numbers THEN
   NOTIFY {{20 is found}}
ENDIF

IF 40 NOT IN $numbers THEN
   NOTIFY {{40 is missing}}
ENDIF

IF 50 ∉ $numbers THEN
   NOTIFY {{50 is missing}}
ENDIF

```

## Emptiness Operators

Emptiness operators in AlgoDraft check whether a set contains no elements. These operators return a **Boolean** result (`TRUE` or `FALSE`) based on the set's state. Using the wrong operand types (e.g., applying `IS EMPTY` to a non-collection type) will raise a `NotSupportedError`.

AlgoDraft provides two forms for emptiness checks:

- **Keyword forms** (`IS EMPTY`, `IS NOT EMPTY`): act as **unary tests** directly on the set.
    
- **Symbolic forms** (`= ∅`, `≠ ∅`): act as **binary comparisons** between the set variable and the **empty set literal** (`∅`).


```
$setA <- ∅   // Or $setA <- CREATE Set<Integer>()
$setB AS Set<Integer> <- {1, 2, 3,}

IF $setA IS EMPTY THEN
   NOTIFY {{$setA is empty}}
ENDIF

IF $setA = ∅ THEN
   NOTIFY {{$setA is empty}}
ENDIF

IF $setB IS NOT EMPTY THEN
   NOTIFY {{$setB is not empty}}
ENDIF

IF $setB ≠ ∅ THEN
   NOTIFY {{$setB is not empty}}
ENDIF
```

## **Set Relation Operators**

Set relation operators in AlgoDraft allow you to compare two sets to determine whether one is a subset or a superset of the other. These operations return a **Boolean** value (`TRUE` or `FALSE`).

AlgoDraft supports both **keyword-based** forms and **symbolic** forms, behaving identically, offering flexibility for different writing styles.

| Syntax                                                   | Meaning                                                                                                     | Example           | Result |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ----------------- | ------ |
| `$setA IS SUBSET OF $setB`  <br>`$setA ⊆ $setB`          | `$setA` is a subset of `$setB` (all elements of `$setA` are in `$setB`; `$setA` may be equal to `$setB`).   | `{1,2} ⊆ {1,2,3}` | `TRUE` |
| `$setA IS PROPER SUBSET OF $setB`  <br>`$setA ⊂ $setB`   | `$setA` is a **proper** subset of `$setB` (all elements of `$setA` are in `$setB`, but `$setA ≠ $setB`).    | `{1,2} ⊂ {1,2,3}` | `TRUE` |
| `$setA IS SUPERSET OF $setB`  <br>`$setA ⊇ $setB`        | `$setA` is a superset of `$setB` (all elements of `$setB` are in `$setA`; `$setA` may be equal to `$setB`). | `{1,2,3} ⊇ {1,2}` | `TRUE` |
| `$setA IS PROPER SUPERSET OF $setB`  <br>`$setA ⊃ $setB` | `$setA` is a **proper** superset of `$setB` (all elements of `$setB` are in `$setA`, but `$setA ≠ $setB`).  | `{1,2,3} ⊃ {1,2}` | `TRUE` |
**Example**
```
$setA AS Set<Integer> <- {1, 2}
$setB AS Set<Integer> <- {1, 2, 3}
$setC AS Set<Integer> <- {1, 2, 3}

IF $setA IS SUBSET OF $setB THEN
    NOTIFY {{$setA is a subset of $setB}}
ENDIF

IF $setA ⊂ $setB THEN
    NOTIFY {{$setA is a proper subset of $setB}}
ENDIF

IF $setB IS SUPERSET OF $setA THEN
    NOTIFY {{$setB is a superset of $setA}}
ENDIF

IF $setC ⊇ $setA THEN
    NOTIFY {{$setC is a superset of $setA}}
ENDIF

IF $setB ⊃ $setA THEN
    NOTIFY {{$setB is a proper superset of $setA}}
ENDIF
```

**Notes**
- A set is always a subset and a superset of itself (`$setA ⊆ $setA` and `$setA ⊇ $setA` are `TRUE`), but **not** a proper subset or proper superset of itself.

- Only use **proper** subset/superset operators when you want to **exclude** equality.

## Set Operations

Set operations in AlgoDraft allow you to combine or compare sets to create new sets based on their relationships. Each operation produces a **new set** as a result, following standard mathematical semantics.

| Operation                | Syntax                                                  | Meaning                                                                       |
| ------------------------ | ------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Union**                | `$setA UNION $setB`  <br>`$setA ∪ $setB`                | Combines all elements from both sets (duplicates removed).                    |
| **Intersection**         | `$setA INTERSECTION $setB`  <br>`$setA ∩ $setB`         | Produces a set of elements common to both sets.                               |
| **Difference**           | `$setA DIFFERENCE $setB`  <br>`$setA - $setB`           | Produces a set of elements in `$setA` but **not** in `$setB`.                 |
| **Symmetric Difference** | `$setA SYMMETRIC DIFFERENCE $setB`  <br>`$setA Δ $setB` | Produces a set of elements in either `$setA` or `$setB`, but **not** in both. |

> Although AlgoDraft supports both **keyword-based** and **symbolic** forms for these operations, they behave identically.

**Examples**
```
$setA AS Set<Integer> <- {1, 2, 3}
$setB AS Set<Integer> <- {3, 4, 5}

$unionSet <- $setA UNION $setB
NOTIFY {{$unionSet is union of $setA and $setB}}      // {1, 2, 3, 4, 5}

$intersectSet <- $setA ∩ $setB
NOTIFY {{$intersectSet is intersection of $setA and $setB}}  // {3}

$differenceSet <- $setA - $setB
NOTIFY {{$differenceSet is difference of $setA and $setB}} // {1, 2}

$symmetricDifferenceSet <- $setA Δ $setB
NOTIFY {{$symmetricDifferenceSet is symmetric difference of $setA and $setB}} // {1, 2, 4, 5}
```

```
$evens <- {2, 4, 6, 8}
$lowPrimes <- {2, 3, 5, 7}

$unionSet <- $evens UNION $lowPrimes     // {2, 3, 4, 5, 6, 7, 8}
$intersectionSet <- $evens INTERSECTION $lowPrimes // {2}
$differenceSet <- $evens DIFFERENCE $lowPrimes // {4, 6, 8}
$symDifferenceSet <- $evens SYMMETRIC DIFFERENCE $lowPrimes // {3, 4, 5, 6, 7, 8}
```

## Adding and Removing Elements from Sets

In AlgoDraft, you can modify an existing set by **adding** or **removing** elements using **Natural Language Descriptions (NLDs)**.  

| Operation             | Syntax                             | Meaning                                                                                                  |
| --------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Add an element**    | `DO {{add $element to $set}}`      | Inserts `$element` into `$set`. If the element is already present, no duplicate is created.              |
| **Add a literal**     | `DO {{add 2 to $set}}`             | Inserts the literal `2` into `$set`.                                                                     |
| **Remove an element** | `DO {{remove $element from $set}}` | Removes `$element` from `$set` if it exists. If the element is not present, the operation has no effect. |

**Example**

```
$numbers AS Set<Integer> <- {1, 3, 5}

// Adding an element variable...
$toAdd <- 2
DO {{add $toAdd to $numbers}}

// Adding a literal value...
DO {{add 4 to $numbers}}

// Removing an element...
$toRemove <- 3
DO {{remove $toRemove from $numbers}}

NOTIFY {{$numbers elements}} // {1, 2, 4, 5}
```
