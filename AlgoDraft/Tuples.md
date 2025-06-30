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

# Type Annotation

Of course. Here is the tutorial for "Tuple Type Annotation" in AlgoDraft, incorporating the `TIMES` keyword syntax for both type annotations and value initialization.

---

**X.Y. Tuple Type Annotation: Specifying Tuple Structures**

**X.Y.1. Introduction to Tuple Annotation**

Because Tuples are a fundamental and versatile data structure in AlgoDraft, the language provides several powerful ways to describe their structure. This is done through **type annotation** when you declare a variable, specify a function parameter, or define a function's return type.

The type annotation for a tuple defines its "shape," primarily its **arity** (the number of elements it holds) and the **data types** of those elements. Choosing the right annotation allows you to specify the structure of a tuple with varying levels of detail and flexibility, making your algorithm designs clearer, more precise, and more robust.

This section introduces the four primary categories of tuple type annotations available in AlgoDraft, highlighting the special `TIMES` keyword which is used to concisely define *homogeneous* tuples (where all elements are of the same type).

---

**X.Y.2. The Four Categories of Tuple Type Annotation**

Here is a brief overview of the four categories, which we will explore in detail:

1.  **Fully Specified:** For tuples where every element's type and position is explicitly listed and potentially different.
2.  **Variadic:** For tuples with a fixed beginning but a variable number of trailing elements.
3.  **Known-Arity Homogeneous:** For tuples containing a *known, fixed number* of elements that are all of the *same type*, defined using the `TIMES` keyword with a literal count.
4.  **Unknown-Arity Homogeneous:** For tuples containing elements all of the same type, where the *number of elements is defined by a variable* at runtime, also using the `TIMES` keyword.

---

**X.Y.3. Category 1: Fully Specified Tuple Types**

This is the most common and fundamental form of tuple annotation. You use it when a tuple serves as a simple, fixed-size record where each element has a distinct and individually known type.

*   **Syntax:**
    `Tuple<Type1, Type2, ..., TypeN>`

*   **Semantics:**
    *   Defines a tuple with exactly `N` elements.
    *   The type of the element at index `0` must be compatible with `Type1`, the element at index `1` with `Type2`, and so on. The types can be different from each other (heterogeneous).
    *   The arity (number of elements) is fixed and known at design time.

*   **Examples:**
    ```algodraft
    // Variable declaration for a 2D point with a label
    pointWithLabel AS Tuple<String, Real, Real> <- ("P1", 10.5, -3.2)

    // Function return type for a standard HTTP-like result
    FUNCTION GetStatus() -> Tuple<Integer, String> // Returns a status code and a message
    :=
        // ... logic to check status ...
        RETURN (200, "OK")
    ENDFUNCTION

    // An empty tuple is specified with no types inside the angle brackets, or no brackets at all.
    emptyTuple AS Tuple <- Tuple()
    ```

---

**X.Y.4. Category 2: Variadic Tuple Types**

This annotation is essential for defining functions that can accept a variable number of arguments, such as a custom logging or formatting function. It specifies a tuple with a fixed beginning followed by an open-ended list of other arguments.

*   **Syntax:**
    `Tuple<Type1, Type2, ..., TrailingType...>`

*   **Semantics:**
    *   The `...` (ellipsis) is a "pack expansion" operator that must be associated with the final type in the list. It signifies "zero or more elements of `TrailingType`."
    *   `ANY...` is the most common form, allowing any type for the variable part.
    *   This defines a tuple with a minimum number of initial elements (e.g., `Type1`, `Type2`) which are then followed by the variable part.

*   **Example (A custom logging function):**
    ```algodraft
    // This function takes a log level, a format string, and a variable number of arguments to format.
    FUNCTION Log(IN details AS Tuple<Integer, String, ANY...>)
    :=
        logLevel AS Integer <- details[0]
        formatString AS String <- details[1]
        // ... logic to format the string using the rest of the tuple's elements ...
        NOTIFY {{Log (Level @logLevel): @(formatted message)}}
    ENDFUNCTION

    // Valid calls would use tuples compatible with this variadic type
    Log((1, "User @s logged in", "Alice"))
    Log((2, "Error code @d from service @s", 503, "AuthService"))
    Log((0, "System initialized.")) // The variadic part is empty, which is valid
    ```

---

**X.Y.5. Category 3: Known-Arity Homogeneous Tuple Types**

Use this annotation as a concise and highly readable way to define a tuple where all elements are of the **same type** and the **number of elements is a known constant** at design time. It serves as a simple, immutable, fixed-size array or vector.

*   **Syntax:**
    `Tuple<ArityLiteral TIMES ElementType>`

*   **Semantics:**
    *   `ArityLiteral` must be a non-negative integer literal (e.g., `3`, `10`).
    *   `TIMES` is a keyword that signifies repetition.
    *   `ElementType` is the single type that all elements in the tuple must have.
    *   This syntax is equivalent to writing the type out long-form (e.g., `Tuple<3 TIMES Real>` is the same as `Tuple<Real, Real, Real>`).

*   **Examples:**
    ```algodraft
    // A 3D vector
    vector3D AS Tuple<3 TIMES Float> <- (1.5, -2.0, 3.0)

    // A 4-element RGBA color value
    rgbaColor AS Tuple<4 TIMES Integer> <- (255, 128, 0, 255)

    // Function signature for a function that requires a pair of integers
    FUNCTION ProcessIntegerPair(IN pair AS Tuple<2 TIMES Integer>)
    :=
        sum AS Integer <- pair[0] + pair[1]
        NOTIFY {{The sum of the pair is @sum}}
    ENDFUNCTION
    ```

---

**X.Y.6. Category 4: Unknown-Arity Homogeneous Tuple Types**

This is the most advanced tuple annotation. It is used when a tuple contains elements of a **single, uniform type**, but its **arity (size) is determined by a variable** during the algorithm's conceptual execution, not at design time. This is a form of **dependent typing**, where a type definition depends on a value.

*   **Syntax:**
    `Tuple<arityVariable TIMES ElementType>`

*   **Semantics:**
    *   `arityVariable` must be a variable identifier that holds a non-negative `Integer` value at runtime.
    *   `TIMES` is the repetition keyword.
    *   `ElementType` is the single type for all elements.
    *   This powerful feature allows your algorithm to have strong type information about tuple sizes even when those sizes are dynamic, making the design more precise and self-documenting.

*   **Example (CSV Parsing):**
    A common use case is parsing data, like from a CSV file, where the number of columns is only known after reading the header row.
    ```algodraft
    // A class to hold the structured CSV data
    CLASS CsvData :=
        CONST N_COLS AS Integer // The number of columns, fixed for this data set
        header AS Tuple<N_COLS TIMES String>
        data AS List<Tuple<N_COLS TIMES String>>
        METHOD NEW(IN num_cols AS Integer) := this.N_COLS <- num_cols ENDMETHOD
    ENDCLASS

    // A function to parse CSV text
    FUNCTION ParseCsvText(IN text AS String) -> CsvData :=
        // 1. Get header line and determine its arity
        headerLine AS String <- DO {{get the first line from @text}}
        // Initially, the result of splitting is just a tuple of strings of some size
        headerTuple AS Tuple<String...> <- DO {{split @headerLine by comma}}

        // 2. Capture the arity in a variable
        numColumns AS Integer <- LENGTH OF headerTuple

        // 3. Now we can work with the dependently-typed tuple
        // Create the main data object, setting its fixed column count
        csvData AS CsvData <- NEW CsvData(numColumns)
        // Conceptually "cast" or assign the header, which now matches the required type
        csvData.header <- headerTuple AS Tuple<numColumns TIMES String>
        csvData.data <- [] // Initialize the list for data rows

        // 4. Process subsequent lines, enforcing the arity
        dataLines AS IIterable<String> <- DO {{get remaining lines from @text}}
        FOR EACH line IN dataLines DO
            rowTuple AS Tuple<String...> <- DO {{split @line by comma}}
            IF LENGTH OF rowTuple != numColumns THEN
                RAISE ERROR {{Row has @(LENGTH OF rowTuple) columns, expected @numColumns}}
            ENDIF
            // Append the now-validated row to the list of data
            csvData.data.Add(rowTuple AS Tuple<numColumns TIMES String>)
        ENDFOR

        RETURN csvData
    ENDFUNCTION
    ```

---

**X.Y.7. The `TIMES` Operator for Value Initialization**

The `count TIMES value` syntax is not only for type annotations but is also a general-purpose expression in AlgoDraft for creating a conceptual sequence of repeated values. Its primary use is for concisely initializing collections like `List`.

*   **Syntax:**
    `countExpression TIMES valueExpression`

*   **Semantics:**
    *   `countExpression` must evaluate to a non-negative `Integer`.
    *   The expression creates a conceptual sequence containing `valueExpression` repeated `countExpression` times, which can then be used to initialize a data structure.

*   **Examples:**
    ```algodraft
    // Initializing a List with 20 zeros
    zeroList AS List<Integer> <- NEW List(20 TIMES 0)

    // Initializing a List with 5 default user objects
    defaultUser AS User <- NEW User("guest", -1)
    userList AS List<User> <- NEW List(5 TIMES defaultUser)

    // Initializing a Tuple directly (less common but possible)
    allFives AS Tuple<4 TIMES Integer> <- (4 TIMES 5) // Creates the tuple (5, 5, 5, 5)
    NOTIFY {{The created tuple is @allFives}}
    ```

---

**X.Y.8. Summary and Choosing the Right Annotation**

To choose the right tuple annotation for your design, consider what you know about the tuple's structure:

*   **Different types and a fixed, known size?**
    *   Use a **Fully Specified** annotation: `Tuple<String, Integer, Boolean>`
*   **The same type and a fixed, known constant size?**
    *   Use a **Known-Arity Homogeneous** annotation: `Tuple<3 TIMES Real>`
*   **The same type, but the size depends on a variable?**
    *   Use an **Unknown-Arity Homogeneous** annotation: `Tuple<n TIMES Real>`
*   **Need to accept a variable number of trailing arguments?**
    *   Use a **Variadic** annotation: `Tuple<String, ANY...>`

Using the most precise annotation possible makes your AlgoDraft designs clearer, more robust, and easier to understand. Additionally, remember that `count TIMES value` is a powerful expression for initializing collections with repeated data.

# Constructing

```
id AS Integer <- INPUT {{the ID of the employee}}
name AS String <- DO {{read the name of @id from the database}}
salary As Real <- DO {{read the salary of @id from the database}}
record AS Tuple<Integer, String, Real> <- Tuple(id, name, salary)
```

```
tupleA AS Tuple<Real, Real> <- Tuple(0.0, 0.0)
tupleB AS Tuple<10 OF Integer> <- Tuple(10 OF 0)

n AS Integer <- INPUT {{an integer}}
tupleC AS Tuple<n OF String> <- Tuple(n OF "")
```
# Operations
## Unpacking
