---
status: Writing
---
The essence of a tuple is a **linear, heterogeneous, immutable, iterable, and duplicable** data structure in AlgoDraft:

- **Linear**: Tuples are organized as a strict **sequence** of elements, forming a clear "chain" where:
    
    - Each element has a **specific position** (first, second, etc.).
        
    - **Predecessor** and **successor** relationships are established by order.
        
    - Elements can be accessed by **zero-based index** (0, 1, 2, …).
        
- **Heterogeneous**: Tuples can contain elements of **different types**. For example, a tuple might hold an integer, a string, and a Boolean side by side without restriction.
    
- **Immutable**: Once created, the contents of a tuple **cannot be changed**. No insertion, deletion, or modification of elements is allowed. Any "change" requires constructing a new tuple.
    
- **Iterable**: Tuples support iteration in the **natural, defined order** of their elements, from the first to the last. Constructs like `FOR EACH` are applicable and follow the tuple’s inherent sequence.
    
- **Duplicable**: Duplicate values are allowed in a tuple. The same element can appear multiple times in different positions without conflict.

They are used to represent a fixed collection of related items where order matters, types may differ, and mutability is sacrificed for **predictability, consistency, and conceptual integrity**.

**Syntax**:

```
CLASS Tuple IMPLEMENTS IROSequentiable<T> :=
ENDCLASS
```
# Type Annotation

Because Tuples are a fundamental and versatile data structure in AlgoDraft, the language provides several powerful ways to describe their structure. This is done through **type annotation** when you declare a variable, specify a function parameter, or define a function's return type.

The type annotation for a tuple defines its "shape," primarily its **arity** (the number of elements it holds) and the **data types** of those elements. Choosing the right annotation allows you to specify the structure of a tuple with varying levels of detail and flexibility, making your algorithm designs clearer, more precise, and more robust.

This section introduces the four primary categories of tuple type annotations available in AlgoDraft, highlighting the special `TIMES` keyword which is used to concisely define *homogeneous* tuples (where all elements are of the same type):

1.  **Fully Specified:** For tuples where every element's type and position is explicitly listed and potentially different.

2.  **Variadic:** For tuples with a fixed beginning but a variable number of trailing elements.

3.  **Known-Arity Homogeneous:** For tuples containing a *known, fixed number* of elements that are all of the *same type*, defined using the `TIMES` keyword with a literal count.

4.  **Unknown-Arity Homogeneous:** For tuples containing elements all of the same type, where the *number of elements is defined by a variable* at runtime, also using the `TIMES` keyword.

## Fully Specified Tuple Types

This is the most common and fundamental form of tuple annotation. You use it when a tuple serves as a simple, fixed-size record where each element has a distinct and individually known type.

**Syntax:**

```
Tuple<Type1, Type2, ..., TypeN>
```

**Semantics:**

*   Defines a tuple with exactly `N` elements.

*   The type of the element at index `0` must be compatible with `Type1`, the element at index `1` with `Type2`, and so on. The types can be different from each other (heterogeneous).

*   The arity (number of elements) is fixed and known at design time.

**Examples:**

```algodraft
// Variable declaration for a 2D point with a label
pointWithLabel AS Tuple<String, Real, Real> <- ("P1", 10.5, -3.2)

// Function return type for a standard HTTP-like result
FUNCTION GetStatus() -> Tuple<Integer, String> := // Returns a status code and a message
	// ... logic to check status ...
	RETURN (200, "OK")
ENDFUNCTION

// An empty tuple is specified with no types inside the angle brackets, or no brackets at all.
emptyTuple AS Tuple <- Tuple()
```

## Variadic Tuple Types

This annotation is essential for defining functions that can accept a variable number of arguments, such as a custom logging or formatting function. It specifies a tuple with a fixed beginning followed by an open-ended list of other arguments.

**Syntax:**

```
Tuple<Type1, Type2, ..., TrailingType...>
```

**Semantics:**

*   The `...` (ellipsis) is a "pack expansion" operator that must be associated with the final type in the list. It signifies "zero or more elements of `TrailingType`."

*   `ANY...` is the most common form, allowing any type for the variable part.

*   This defines a tuple with a minimum number of initial elements (e.g., `Type1`, `Type2`) which are then followed by the variable part.

**Example (A custom logging function):**

```algodraft
// This function takes a log level, a format string, and a variable number of arguments to format.
FUNCTION Log(IN details AS Tuple<Integer, String, ANY...>) :=
	logLevel AS Integer <- details[0]
	formatString AS String <- details[1]
	args AS Tuple<ANY...> <- details[2 .. ]
	msg AS String <- DO {{format @formatString using @args}}
	NOTIFY {{@msg}}
ENDFUNCTION

// Valid calls would use tuples compatible with this variadic type
Log((1, "User @s logged in", "Alice"))
Log((2, "Error code @d from service @s", 503, "AuthService"))
Log((0, "System initialized.")) // The variadic part is empty, which is valid
```

## Known-Arity Homogeneous Tuple Types

Use this annotation as a concise and highly readable way to define a tuple where all elements are of the **same type** and the **number of elements is a known constant** at design time. It serves as a simple, immutable, fixed-size array or vector.

**Syntax:**

```
Tuple<ArityLiteral TIMES ElementType>
```

**Semantics:**

*   `ArityLiteral` must be a non-negative integer literal (e.g., `3`, `10`).

*   `TIMES` is a keyword that signifies repetition.

*   `ElementType` is the single type that all elements in the tuple must have.

*   This syntax is equivalent to writing the type out long-form (e.g., `Tuple<3 TIMES Real>` is the same as `Tuple<Real, Real, Real>`).

**Examples:**

```algodraft
// A 3D vector
vector3D AS Tuple<3 TIMES Float> <- (1.5, -2.0, 3.0)

// A 4-element RGBA color value
rgbaColor AS Tuple<4 TIMES Integer> <- (255, 128, 0, 255)

// Function signature for a function that requires a pair of integers
FUNCTION ProcessIntegerPair(IN pair AS Tuple<2 TIMES Integer>) :=
	sum AS Integer <- pair[0] + pair[1]
	NOTIFY {{The sum of the pair is @sum}}
ENDFUNCTION
```

## Unknown-Arity Homogeneous Tuple Types

This is the most advanced tuple annotation. It is used when a tuple contains elements of a **single, uniform type**, but its **arity (size) is determined by a variable** during the algorithm's conceptual execution, not at design time. This is a form of **dependent typing**, where a type definition depends on a value.

**Syntax:**

```
Tuple<arityVariable TIMES ElementType>
```

**Semantics:**

*   `arityVariable` must be a variable identifier that holds a non-negative `Integer` value at runtime.

*   `TIMES` is the repetition keyword.

*   `ElementType` is the single type for all elements.

*   This powerful feature allows your algorithm to have strong type information about tuple sizes even when those sizes are dynamic, making the design more precise and self-documenting.

**Example (CSV Parsing):**

A common use case is parsing data, like from a CSV file, where the number of columns is only known after reading the header row.

```algodraft
// A class to hold the structured CSV data
CLASS CsvData :=
	CONST N_COLS AS Integer, // The number of columns, fixed for this data set
	header AS Tuple<N_COLS TIMES String>,
	data AS List<Tuple<N_COLS TIMES String>>,
	
	METHOD NEW(num_cols AS Integer) := this.N_COLS <- num_cols ENDMETHOD
ENDCLASS


FUNCTION ParseCsvText(text AS String) -> VpnGateCsv :=
    lines AS Tuple<String...> <- DO {{remove leading and trailing
        whitespace from @text & split it around newline characters}}
    linesIter AS Iterator<String> <- ITERATOR OF lines
    // Parsing the first line as header...
    IF linesIter EXHAUSTED THEN
        RAISE {{header eror: no header was found}}
    ENDIF
    line <- NEXT linesIter
    header AS Tuple<String...> <- DO {{split @line around ','}}
    nCols AS Integer <- LENGTH OF header
    csvData AS CsvData <- NEW CsvData(nCols)
    csvData.header <- header
    // Parsing the rest of lines...
    nDataLine AS Integer
    record AS Tuple<nCols TIMES String>
    FOR EACH line IN linesIter DO
        dataLine AS Tuple<String...> <- DO {{split @line around ','}}
        nDataLine <- LENGTH OF dataLine
        diff AS Integer <- nCols - nDataLine
        IF diff < 0 THEN
            RAISE {{format error: inconsistent column numbers}}
        ELSE IF diff > 0 THEN
            record <- dataLine + Tuple(diff TIMES "")
        ELSE
            record <- dataLine
        ENDIF
        DO {{append @record to @vpnGateCsv.data}}
    ENDFOR
    RETURN csvData
ENDFUNCTION
```

## The `TIMES` Operator for Value Initialization

The `count TIMES value` syntax is not only for type annotations but is also a general-purpose expression in AlgoDraft for creating a conceptual sequence of repeated values. Its primary use is for concisely initializing collections like `List`.

**Syntax:**

```
countExpression TIMES valueExpression
```

**Semantics:**

*   `countExpression` must evaluate to a non-negative `Integer`.

*   The expression creates a conceptual sequence containing `valueExpression` repeated `countExpression` times, which can then be used to initialize a data structure.

**Examples:**

```algodraft
// Initializing a List with 20 zeros...
zeroList AS List<Integer> <- NEW List(20 TIMES 0)

// Initializing a List with 5 default user objects...
defaultUser AS User <- NEW User("guest", -1)
userList AS List<User> <- NEW List(5 TIMES defaultUser)

// Initializing a Tuple directly (less common but possible)
allFives AS Tuple<4 TIMES Integer> <- (4 TIMES 5) // Creates the tuple (5, 5, 5, 5)
NOTIFY {{The created tuple is @allFives}}
```

## Summary and Choosing the Right Annotation

To choose the right tuple annotation for your design, consider what you know about the tuple's structure:

*   **Different types and a fixed, known size?**
    *   Use a **Fully Specified** annotation: `Tuple<String, Integer, Boolean>`
*   **The same type and a fixed, known constant size?**
    *   Use a **Known-Arity Homogeneous** annotation: `Tuple<3 TIMES Real>`
*   **The same type, but the size will be known during the execution?**
    *   Use an **Unknown-Arity Homogeneous** annotation: `Tuple<n TIMES Real>`
*   **Need to accept a variable number of trailing arguments?**
    *   Use a **Variadic** annotation: `Tuple<String, ANY...>`

Using the most precise annotation possible makes your AlgoDraft designs clearer, more robust, and easier to understand. Additionally, remember that `count TIMES value` is a powerful expression for initializing collections with repeated data.

# Instantiation

Empty tuples:

```
empty1 AS Tuple <- Tuple()
empty2 As Tuple <- (,)
```

Using literal:

```
id AS Integer <- INPUT {{the ID of the employee}}
name AS String <- DO {{read the name of @id from the database}}
salary As Real <- DO {{read the salary of @id from the database}}
record AS Tuple<Integer, String, Real> <- (id, name, salary,)
```

Using `TIMES` operator:

```
tupleA AS Tuple<Real, Real> <- Tuple(0.0, 0.0)
tupleB AS Tuple<10 OF Integer> <- Tuple(10 TIMES 0)

n AS Integer <- INPUT {{an integer}}
tupleC AS Tuple<n OF String> <- Tuple(n TIMES "")

// Initializing a Tuple directly (less common but possible)
allFives AS Tuple<4 TIMES Integer> <- (4 TIMES 5) // Creates the tuple (5, 5, 5, 5)
NOTIFY {{The created tuple is @allFives}}
```

Using iterables:

```
text AS String <- INPUT {{some text}}
strTuple AS Tuple<Char...> <- Tuple(text)

myList AS List<Integer> <- INPUT {{some in tegers}}
intTuple AS Tuple<Integer...> <- Tuple(myList)
```
# Operations
## Unpacking


## Appending, Prepending, and Inserting

**Syntax**:

```
newTuple AS Tuple<Type...> <- APPEND iterable TO tuple
newTuple AS Tuple<Type...> <- PREPEND iterable TO tuple
newTuple AS Tuple<Type...> <- INSERT iterable INTO tuple AT index
```

**Example**:

```
start AS Tuple<Integer> <- (10, 20)
end AS Tuple<Integer> <- (40, 50)

// Append single element
newTuple1 AS Tuple<Integer...> <- APPEND (30,) TO start // (10, 20, 30)

// Prepend single element
newTuple2 AS Tuple<Integer...> <- PREPEND (0,) TO start // (0, 10, 20)

// Append an iterable (another tuple) - this is concatenation
newTuple3 AS Tuple<Integer...> <- APPEND end TO start // (10, 20, 40, 50)

// Prepend an iterable
newTuple4 AS Tuple<Integer...> <- PREPEND start TO end // (10, 20, 40, 50)
```