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
# Constructing
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
## Equality/Inequality Operators
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
These binary operators perform a check, returning a Boolean value, to determine if the left operand is considered (IN, ∈) or is not considered (NOT IN, ∉) a member (an element) of the right operand, based on mathematical equality. These operators are typically used within conditional statements to control algorithmic flow based on an element's presence or absence in a set.

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
