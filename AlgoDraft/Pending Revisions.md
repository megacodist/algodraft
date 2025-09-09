---
status: Writing
---
# `ALIAS`

## New Syntax

```
ALIAS
	<type> <comma_sep_names> FROM <module>;
	<type> <comma_sep_names> FROM <module>;
	...
ENDALIAS
```

* **`<type>`**: can be `CONST`, `CLASS`, `FUNCTION`, `INTERFACE`
* **`<comma_sep_names>`**: the comma separated names of type `<type>`
* **`<module>`**: 
* **`;`**: 

## Generic Aliases

Should we accept generic aliases?

```
ALIAS Sorter<T> FOR Function<(T, T) -> Integer>
```

**Example:**
```algodraft
// Define a clear, generic alias for a comparison function for any type T
// (This assumes AlgoDraft supports generics in ALIAS definitions)
ALIAS Sorter<T> FOR Function<(T, T) -> Integer>

// Use the alias in a class constructor signature for clarity
CLASS SortedList<T> :=
    // ...
    METHOD NEW(sorter AS Sorter<T> OR NULL <- NULL)
    // ...
ENDCLASS

// Use a concrete instantiation of the alias for a variable declaration
myCustomIntSorter AS Sorter<Integer> <- SomeIntegerSortFunction // Assign a compatible function
```

# `CATCH`

Catching two or more errors in the same `CATCH` clause using `OR` operator:

```
bkpConfig AS VpnConfig <- this.config
TRY
	this.config.UpdateWith(other.config)
CATCH {{mismatching config names}} OR {{conflicting config IDs}} DO
	this.config <- bkpConfig
	DO {{Re-raise the error}}
ENDTRY
```

# `CLASS`

1. How about replacing `NEW` for `CONSTRUCTOR` in a class for constructor definition?
2. Ending data definition in a class with a semicolon (`;`).

# Casting

What is casting and how should it be implemented?

Good syntax? `CAST obj TO DataType`

# `CONCEPT`

I would like the `CONCEPT` block to enable me to:
1. Sketch the API of a conceptual type only.
2. Or, define the behavio

**Example 1: Sketching the API of Singly-Linked Lists**

```
// This Singly-Linked Node (`SlNode`) is the building block of a
// Singly-Linked List (`SlList`).
CLASS SlNode<T> :=
	data AS T;
	next AS SlNode<T> OR NULL;

	METHOD NEW(data AS T, next AS SlNode<T> OR NULL) :=
		this.data <- data;
		this.next <- next;
	ENDMETHOD
ENDCLASS


CONCEPT
	// This singly-linked list (`SlList`) class offers universally
	// understandable, bug-free API to work on singly-directed chain of
	// nodes.
	CLASS SlList<T> :=
		PRIVATE head AS SlNode<T> OR NULL;

        METHOD GetHeadNode() -> SlNode<T> OR NULL;
        METHOD InsertAfter(node AS SlNode<T>, data AS T) -> SlNode<T>;
        METHOD InsertAsHead(data AS T) -> SlNode<T>;
        METHOD DelAfter(node AS SlNode<T>) -> NULL;
        METHOD DelHead() -> NULL;
        METHOD Find(data AS T) -> SlNode<T> OR NULL;
	ENDCLASS
ENDCONCEPT
```


**Example 2: Defining the API of Singly-Linked Lists**

```
// This Singly-Linked Node (`SlNode`) is the building block of a
// Singly-Linked List (`SlList`).
CLASS SlNode<T> :=
	data AS T;
	next AS SlNode<T> OR NULL;

	METHOD NEW(data AS T, next AS SlNode<T> OR NULL) :=
		this.data <- data;
		this.next <- next;
	ENDMETHOD
ENDCLASS


CONCEPT
	// This singly-linked list (`SlList`) class offers universally
	// understandable, bug-free API to work on singly-directed chain of
	// nodes.
	CLASS SlList<T> :=
		PRIVATE head AS SlNode<T> OR NULL;

        METHOD GetHeadNode() -> SlNode<T> OR NULL := {{
        	Returns the head node. If the list is empty returns `NULL`.
        	So, this method can be used for checking emptyness of the
        	list.
        	}}
        ENDMETHOD
        METHOD InsertAfter(
        		data AS T,
        		node AS SlNode<T>,
        		) -> SlNode<T> := {{
        	Creats a node after `node` with `data` in it.
        	}}
        METHOD InsertAsHead(data AS T) -> SlNode<T> := {{
        	Creates a new siggly-linked node as head with `data` in it. so,
        	the current head will lie after it.
        	}}
        ENDMETHOD
        METHOD DelAfter(node AS SlNode<T>) -> NULL := {{
        	Deletes the node after the provided node of `node`. If it is
        	already the tail (its `next` is `NULL`), it does nothing.
        	}}
        ENDMETHOD
        METHOD DelHead() -> NULL := {{
        	Deletes the head node.
        	}}
        ENDMETHOD
        METHOD Find(
        		data AS T,
        		start AS SlNode<T> OR NULL <- NULL,
        		) -> SlNode<T> OR NULL := {{
        	Finds the first occurrence of a node with data as `data` from
        	the `start` node if it is not `NULL` otherwise starts from the
        	head.
        	}}
        ENDMETHOD
	ENDCLASS
ENDCONCEPT
```


# Constants

How about forcing constant names to be obligated to consist of two or more underscore connected parts?

# Downcasing

One of the use cases of `IS A/AN` operator mentioned in AlgoDraft is downcasing. Please explain it more.

# `Function` Type and Function Statement

Function signature of a function statement and generic part of `Function` type are not completely match each other. Is this a bad design choice?

# Functions

How about adding variable number of arguments in function signature?

```
FUNCTION FormatString(fmt AS String, args AS Any...) -> NULL :=
ENDFUNCTION
```

In this example, `FormatString` function accept a formatter (`fmt`) and a variable number of arguments to format the string.

```
FUNCTION Sum(nums AS Float...) -> Float :=
ENDFUNCTION
```

This function accepts a variable number of real numbers to sum.

1.  **Rule:** The variadic parameter must be the **last parameter** in the function's parameter list.
    *   `FUNCTION Log(IN format AS String, IN values AS ANY...)` -> **VALID**
    *   `FUNCTION Bad(IN values AS ANY..., IN format AS String)` -> **INVALID**

2.  **Internal Type:** Inside the function, the variadic parameter (e.g., `values`) is treated as a tuple. A `Tuple` is immutable, which is often a safer choice for incoming arguments. So, `values AS ANY...` would mean that inside the function, `values` is of type `Tuple<ANY...>`. `nums AS Float...` would mean `nums` is of type `Tuple<Float...>`.

3.  **Calling Convention:**
    *   The compiler/interpreter/conceptual model understands that when a function with a variadic parameter is called, all extra positional arguments are collected into a tuple and assigned to that parameter.
    *   `Log("User: %s, ID: %d", "Alice", 101)`
        *   `format` gets `"User: %s, ID: %d"`.
        *   `values` becomes the tuple `("Alice", 101)`.


# Index Basic Type

How about adding a basic type named `Index` very like `Integer` but contains two special values `FIRST` and `LAST` to work with indexable data structures.

```
lines AS Tuple<String...> <- INPUT {{some lines, might start and end with '*'}}
// Removing possible comments (starting with *)...
startIdx AS Index <- FIRST + 1 IF {{@lines[FIRST] starts with '*'}}	ELSE FIRST
stopIdx AS Index <- LAST - 1 IF {{@lines[LAST] starts with '*'}} ELSE LAST
lines <- lines[startIdx .. stopIdx]
```

# `IS A/AN`

Right now, this operator only works on objects as follow:

```
<obj> IS A/AN <type_interface>
```

1. `<obj>` is an object. How about overloading this operator by allowing the left-hand operator to be both an object or a type object?
2. Checking against two or more types: How aout this?
	```
	<obj_or_type> IS A/AN (<type_1> OR <type_2> OR ... OR <type_N>)
	```

# IContainer

How about adding a postfix unary operator for checking emptiness?

```AlgoDraft
CLASS IContainer INHERITS IIterable :=
	OPERATOR (this IS EMPTY) -> Boolean	ENDOPERATOR
ENDCLASS
```

Needless to say that the negative form is `<container> IS NOT EMPTY`.

## Checking empty

**Syntax**:

```
OPERATOR this IS EMPTY -> Boolean
ENDOPERATOR
```

**Semantics**:

Checks whether this container is empty or not.
# IIterable Interface

How about making the syntax generic (adding `<T>`)?

```
INTERFACE IIterable<T> :=
    OPERATOR ITERATOR OF this -> IIterator<T>;
ENDINTERFACE
```

# `Iterator` Basic Type

How about to an interface named `IIterator<T>` as follow:

```
INTERFACE IIterator<T> :=
	OPERATOR this EXHAUSTED -> Boolean;
	OPERATOR NEXT this -> T;
ENDINTERFACE
```

**Example**:

```
// This Singly-Linked Node (`SlNode`) is the building block of a
// Singly-Linked List (`SlList`).
CLASS SlNode<T> :=
	data AS T;
	next AS SlNode<T> OR NULL;

	METHOD NEW(data AS T, next AS SlNode<T> OR NULL) :=
		this.data <- data;
		this.next <- next;
	ENDMETHOD
ENDCLASS


CLASS SlIter IMPLEMENTS IIterator<T> :=
	PRIVATE curr AS SlNode OR NULL;

	METHOD NEW(head AD SlNode OR NULL) :=
		this.curr <- head;
	ENDMETHOD
	OPERATOR this EXHAUSTED -> Boolean :=
		RETURN this = NULL;
	ENDOPERATOR
	OPERATOR NEXT this -> T :=
		IF this EXHAUSTED THEN
			RAISE {{exhaustion}};
		ENDIF
		temp AS T <- this.curr.data;
		this.curr <- this.curr.next;
		RETURN temp;
	ENDOPERATOR
ENDCLASS


CONCEPT
	// This singly-linked list (`SlList`) class offers universally
	// understandable, bug-free API to work on singly-directed chain of
	// nodes.
	CLASS SinglyLinkedList<T> IMPLEMENTS IIterable<T> :=
		PRIVATE n AS Integer;
		PRIVATE head AS SlNode<T> OR NULL;

		METHOD NEW();
		METHOD ITERATOR OF this -> SlIter<T>;
		METHOD GetSize() -> Integer;
    	METHOD IsEmpty() -> BOOLEAN;
        METHOD GetHeadNode() -> SlNode<T> OR NULL;
        METHOD InsertAfter(node AS SlNode<T>, data AS T) -> SlNode<T>;
        METHOD InsertAsHead(data AS T) -> SlNode<T>;
        METHOD DelAfter(node AS SlNode<T>) -> NULL;
        METHOD DelHead() -> NULL;
        METHOD Find(data AS T) -> SlNode<T> OR NULL;
	ENDCLASS
ENDCONCEPT
```

# IMPORT

How about the following syntax?

```
IMPORT
	CLASS ClassName1 [AS CustomName1], ClassName2 FROM <a_module>;
	FUNCTION
	CONSTANT
	INTERFACE
ENDIMPORT
```

# Mapping

There are two mapping annotations:
1. `Mapping<KT TO VT>`
2. `Mapping<KT -> VT>`

# Numeric Basic Type

I decided to have `±INFINITY`/`±∞` conceptual values in numeric types. So, their possible operations must be defined in `Numeric` class.

# RECORD

How about a one-line (or sometimes inline) version of RECORD syntax.

We believe that distinction between `RECORD`s and `CLASS`es are not very good. A simple data aggregate (`RECORD`) needs comparing for equality (`=`) and this means operator definition in a `RECORD` which by default is forbidden. So it is a good idea to gather all user-defined types under the same umbrella of CLASS`.

# Sequence Slicing

Changing the slicing syntax to `[a .. b Δ c]` must be considered.
How about the following open-ended notations? `[.. b]` and `[a ..]`

# Sequences vs Indexables

Sequences introduce the following extensions to their indexables counterparts:

1. **Comparability**: They implement `IComparable` with the default comparing behavior of lexicographical order:
2. **Slicing**

```
INTERFACE IROSequentiable INHERITS IROIndexable, IComparable :=
    OPERATOR this[interval AS ISteppedIntStream] -> IROSequentiable
ENDINTERFACE
```

# Sigil

The preceding character in variable and constant names are called sigil. In this proposed syntax:

- **Variables:** Remove `$` from declaration and usage examples. Syntax becomes `varName AS DataType [<- initialValue]`. Explain that variables are now plain identifiers. 

- **Constants:** Remove `$` from declaration and usage examples. Syntax becomes `CONST CONST_NAME AS DataType <- value`. Explain that constants are now plain identifiers (typically all caps by convention).

- Explain that all AlgoDraft identifiers (variables, constants, type names, function names, interface names, etc.) when referred to within an NLD **must** be prefixed with `@`. 
- Provide clear examples: `NOTIFY {{user @userName (type: @UserRecord) logged in using @LoginFunction}}`.
- Reiterate that outside NLDs, these identifiers are used without the `@`.
- Update "Key Characteristics of NLDs" to reflect this: "Integration of Identifiers: NLDs explicitly reference AlgoDraft identifiers by prefixing them with `@` (e.g., `@userCount,` `@ProcessData`, `@ErrorType`)."
- Update "NLDs Best Practices" if needed.

# Statements

How about this: it is a good practice to end every statement which does not end with an `END` keyword with a semicolon (`;`)?

# String

1. A syntax is needed to split across multiple lines. How about this?

	```
	msg <- ("bad MOC: expected a paragraph inside %s but got"
        " nothing").Format(mocListItem);
	```

# Tuple

Variadic syntax of tuple type annotation is start-anchored: `Tuple<Type1, Type2, ..., TrailingType...>`. 

How about defining:

* End-anchored syntax like `Tuple<LeadingType..., Type1, Type2>`.

* Both-end-anchored syntax like `Tuple<Type1, MiddleType..., Type2>`.

* Double (or even multiple) variadic syntax like `Tuple<Integer..., String...>`

# Variables

How about the followings?
1. One or more variables on one declaration:
	```
	var1, var2, ..., varN AS DataType;
	```
2. Ending declarations with semicolon:
	```
	var1, var2, ..., varN AS DataType;
	```