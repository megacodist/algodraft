The `IContainer` interface in AlgoDraft defines the **fundamental contract** for any data structure that serves as a collection or "container" of elements. This interface ensures that, at a minimum, any type that claims to be a container will provide a way to:

1. Determine the number of elements it currently holds (its size or length).

2. Check if a specific element is present within it (membership testing).

To effectively perform membership testing, a container must allow access to its elements. Therefore, `IContainer` builds upon (extends) the `IIterable` interface, ensuring that all containers can be iterated over. Furthermore, the membership test (`IN` operator) will rely on the concept of equality, ideally defined by the `IEquatable` interface for the elements being compared.

The `IContainer` interface provides a universal way to interact with the most basic properties of diverse collection types found in AlgoDraft.

**Syntax:**

```
// Assumed prerequisite interface:
// INTERFACE IIterable
//     OPERATOR ITERATOR OF this -> Iterator ENDOPERATOR // Iterator is the basic iterator type
// ENDINTERFACE

INTERFACE IContainer INHERITS IIterable :=
    // Operator to check if an element is present within the container.
    // Relies on the container being IIterable (inherited contract) and its elements
    // (or the element being checked) supporting an equality comparison.
    OPERATOR (element AS ANY) IN this -> Boolean ENDOPERATOR

    // Operator to get the total number of elements in the container.
    OPERATOR LENGTH OF this -> Integer ENDOPERATOR

    // OPERATOR ITERATOR OF this -> Iterator ENDOPERATOR is inherited from IIterable.
ENDINTERFACE
```

- The `INHERITS IIterable` clause means that any type implementing `IContainer` must also fulfill the contract of `IIterable`, notably providing an `ITERATOR OF this` operator.

- `OPERATOR element IN this -> Boolean`: This defines the membership testing operator.

- `OPERATOR LENGTH OF this -> Integer`: This defines the operator for getting the container's size.

# The `IN` Operator (Membership Testing)

The `IN` operator checks if a given element is present within a container.

**Syntax:**

```
elemToCheck IN containerVar
```

**Semantics:**

The `IN` operator evaluates to a Boolean (`TRUE` or `FALSE`). It works by **iteration for equality under the hood**:

1. **Iteration:** It first obtains an iterator from `containerVar` using the `ITERATOR OF` operator (inherited from the `IIterable` interface). It then uses this iterator to access each `containerElem`. The order of access depends on the `containerVar`'s specific iterator implementation (e.g., sequential for a `List`, potentially indeterministic for a `Set`).

2. **Equality Check:** For each `containerElem` obtained from the iterator, an equality comparison is conceptually performed: `elemToCheck = containerElem`.
    
    - This comparison uses the rules defined by the `IEquatable` interface.
        
        - Many of AlgoDraft builtin types (like `Integer`, `String`) implement `IEquatable` and use standard value equality.
        
        - For instances of user-defined `CLASS`es, their implemented `=` operator (if they `IMPLEMENTS IEquatable`) is used.
    
    - **Handling Comparison Outcomes:**
        
        - If `elemToCheck = containerElem` evaluates to `TRUE`, the IN operator immediately stops iterating and returns `TRUE`.
        
        - If it evaluates to `FALSE`, the iteration continues.
        
        - If the equality comparison `elemToCheck = containerElem` itself would **raise an error** (e.g., because no `IEquatable` contract allows for a meaningful comparison between their specific types), the `IN` operator **treats this specific failed comparison as if it were `FALSE`** and continues iterating through the container. The `IN` operator itself does not propagate this equality error; its purpose is to determine presence based on successful equality.

3. **Final Result:**
    
    - If the iteration completes and no equality comparison has returned `TRUE` (meaning all comparisons were `FALSE` or were treated as `FALSE` due to an inability to compare), the `IN` operator returns `FALSE`.

**Example:**

```
// Assume Point class implements IEquatable...
// Assume UnrelatedClass does NOT implement IEquatable...
CLASS Point IMPLEMENTS IEquatable :=
	x AS Integer,
	y AS Integer,
	
	{{The implementation of IEquatable interface goes here}}
	
	{{The possible implementation of other methods and operators goes here}}
ENDCLASS

CLASS UnrelatedClass :=
	id AS String,
	
	{{The possible implementation of other methods and operators goes here}}
ENDCLASS

p1 AS Point <- NEW Point(10, 20)
p2 AS Point <- NEW Point(30, 40)
p3 AS Point <- NEW Point(10, 20) // Equal to p1 by value

u1 AS UnrelatedClass <- NEW UnrelatedClass("UID1")

myPoints AS List<Point> <- [p1, p2]
// myMixedBag can hold items of ANY type
myMixedBag AS List<ANY> <- [p1, u1, "text_value"]

IF p3 IN myPoints THEN
    NOTIFY {{point (@(p3.x), @(p3.y)) found in @myPoints}} // This will notify
ENDIF

searchPoint AS Point <- NEW Point(50, 60)
IF searchPoint IN myPoints THEN
    // This block won't execute
ELSE
    NOTIFY {{point (@(searchPoint.x), @(searchPoint.y)) not found in @myPoints}}
ENDIF

// Behavior with uncomparable items in a mixed list...
searchUnrelated AS UnrelatedClass <- NEW UnrelatedClass("UID1") // Create another instance to search for

IF searchUnrelated IN myMixedBag THEN
    // This block will NOT execute.
    // 1. (searchUnrelated = p1) -> '=' would error (Point vs UnrelatedClass, assume no cross-type equality defined), IN treats as FALSE.
    // 2. (searchUnrelated = u1) -> '=' would error (UnrelatedClass not IEquatable), IN treats as FALSE.
    // 3. (searchUnrelated = "text_value") -> '=' would error, IN treats as FALSE.
    NOTIFY {{@searchUnrelated was found in @myMixedBag. this indicates unexpected behavior or default equality.}}
ELSE
    NOTIFY {{@searchUnrelated was not found in @myMixedBag, due to absence or comparison issues being treated as non-matches}}
ENDIF

IF "text_value" IN myMixedBag THEN
    NOTIFY {{string "text_value" found in @myMixedBag}} // This will notify ("text_value" = "text_value" is TRUE)
ENDIF
```

# The `LENGTH OF` Operator (Getting Size)

The `LENGTH OF` operator provides the number of elements currently in a container.

**Syntax:**

```
LENGTH OF containerVar
```

**Semantics:**

- Evaluates to a non-negative Integer representing the count of elements in the `containerVar`.

- Returns 0 if the container is empty.

**Example:**

```
activeItems AS List<String> <- ["alpha", "beta", "gamma"]
itemCount AS Integer <- LENGTH OF activeItems
NOTIFY {{the collection @activeItems contains @itemCount items}}
// User is informed that the collection activeItems contains 3 items

emptyCollection AS Set<Integer> <- {} // Assuming {} is syntax for an empty set
sizeOfEmpty AS Integer <- LENGTH OF emptyCollection
NOTIFY {{the empty collection @emptyCollection has @sizeOfEmpty elements}}
// User is informed that the empty collection emptyCollection has 0 elements
```

# `IContainer` Use Cases

1. **Fundamental Abstraction:** `IContainer` provides a common, minimal way to interact with any collection, irrespective of its specific underlying structure (e.g., `List`, `Set`, `Tuple`) or other specialized capabilities.

2. **Polymorphism:** It enables the creation of generic functions or algorithms that can operate on any data structure that implements `IContainer`. These functions can query the size or check for item presence without needing to know the collection's concrete type.

```
FUNCTION ReportContainerStatus(dataSet AS IContainer, itemToQuery AS ANY) :=
    dataSize AS Integer <- LENGTH OF dataSet
    NOTIFY {{the dataset @dataSet currently holds @dataSize elements}}

    IF itemToQuery IN dataSet THEN
        NOTIFY {{the item @itemToQuery is present in @dataSet}}
    ELSE
        NOTIFY {{the item @itemToQuery is NOT present in @dataSet}}
    ENDIF
ENDFUNCTION

// Example usage
userList AS List<String> <- ["Alice", "Bob"]
ReportContainerStatus(userList, "Alice")

idSet AS Set<Integer> <- {101, 202}
ReportContainerStatus(idSet, 303)
```

3. **Basis for Other Interfaces:** As `IContainer` itself extends `IIterable`, it serves as a foundational interface. More specialized collection interfaces (like `IImmutableSequence`, `IMutableSequence`, etc.) can, in turn, extend `IContainer` to inherit its basic size, iteration, and membership checking capabilities, then add their own specific contracts.

The `IContainer` interface is a core contract in AlgoDraft for all collection types. By extending `IIterable`, it ensures that any container can be iterated over, can report its length, and can test for element membership using the `IN` operator. The `IN` operator leverages iteration and the equality mechanisms (ideally via `IEquatable` on element types) to determine presence, gracefully handling undefined comparisons internally by treating them as non-matches. This interface is key to writing generic and polymorphic algorithms that operate on collections.
