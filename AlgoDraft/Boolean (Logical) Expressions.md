---
status: Writing
---
A **Boolean expression** (also called a logical expression) is a fundamental concept in AlgoDraft. It's any statement or combination of elements that ultimately evaluates to a single Boolean value: either **`TRUE`** or **`FALSE`**. These expressions are the core mechanism for making decisions and controlling the flow within your algorithms.

Think of a Boolean expression as asking a question that has a yes/no answer. Based on the answer (`TRUE` or `FALSE`), your algorithm can decide what to do next.

Here's how Boolean expressions are formed in AlgoDraft, following a clear set of rules:

**Rule 1: Boolean literals are Boolean expressions:** The simplest Boolean expressions are the literal values themselves.
	
* `TRUE` (This expression evaluates directly to `TRUE`)
	
* `FALSE` (This expression evaluates directly to `FALSE`)

**Rule 2: Boolean variables are Boolean expressions:** If you have declared a variable of type Boolean, using that variable name (say $variableName) constitutes a Boolean expression. The expression evaluates to the current value (`TRUE` or `FALSE`) stored in that variable.

```
$isDataValid AS Boolean
$isDataValid <- TRUE

// Later in the code...
// Using the variable here is a Boolean expression evaluating to TRUE...
// ... $isDataValid ...
```

**Rule 3: Any operation (function, method, operator) that returns a Boolean value is a Boolean expression:** Many operations in AlgoDraft don't store Booleans directly but produce them as a result. The operation itself then acts as a Boolean expression. Common examples include:

 * **Comparison Operators:** Comparing objects implementing comparison operators ([[IEquatable Interface]] or [[IComparable Interface]]) results in `TRUE` or `FALSE`.
	
   ```
    $userAge AS Integer <- 25
    // The following comparisons are Boolean expressions:
    $userAge >= 18       // Evaluates to TRUE
    $userAge == 30       // Evaluates to FALSE
    ```

*   **Function/Method Calls:** Calling a function or method specifically designed to return a `Boolean` value.

```AlgoDraft
$myList AS List<Integer> <- NEW List<Integer>
DO {{is $myList empty?}}  // Evaluates to TRUE or FALSE depending on the list's state
	
// Assume a function IsValidEmail exists:
$email AS String <- "test@example.com"
// The following function call is a Boolean expression:
IsValidEmail($email) // Evaluates to TRUE or FALSE
```

**Rule 4: Any well-formed combination of the above entities with Boolean operators, is a Boolean expression:** If we have two Boolean expressions, the followings are Boolean expressions as well:

* The negation of each of them
* The Boolean AND of both of the
* The Boolean XOR of both of them
* The Boolean OR of both of them

```algodraft
$isLoggedIn AS Boolean <- TRUE
$isAdmin AS Boolean <- FALSE
$accountActive AS Boolean <- TRUE
$retryCount AS Integer <- 2

// Combining variables and comparison results with logical operators:
$isLoggedIn AND $isAdmin                 // Evaluates to FALSE
NOT $isAdmin                              // Evaluates to TRUE
$isLoggedIn OR $isAdmin                 // Evaluates to TRUE
($retryCount < 3) AND $accountActive    // Evaluates to TRUE (TRUE AND TRUE)
```

# Operator Precedence in Boolean Expressions

When you combine multiple Boolean operators without using parentheses, AlgoDraft evaluates them in a specific default order, known as **operator precedence**. This is similar to how multiplication is done before addition in arithmetic (`*` before `+`).

The precedence order for AlgoDraft Boolean operators is:

1.  **`NOT`** (Highest precedence - evaluated first)
2.  **`AND`**
3.  **`XOR`**
4.  **`OR`** (Lowest precedence - evaluated last)

**Example:**

Consider the expression: `NOT $a OR $b AND $c XOR $d`

Based on precedence, it would be evaluated as if grouped like this:
`(NOT $a) OR (($b AND $c) XOR $d)`

1.  `NOT $a` is calculated first.
2.  `$b AND $c` is calculated next.
3.  The result of `$b AND $c` is then `XOR`ed with `$d`.
4.  Finally, the result of `NOT $a` is `OR`ed with the result of the `XOR` operation.

# Importance of Parentheses

While understanding precedence is important, relying solely on it can make complex expressions difficult to read and prone to errors.

**Best Practice:** Use parentheses, `()`, liberally to explicitly group parts of your Boolean expressions. This:

*   **Ensures Correctness:** Guarantees the expression is evaluated in the order you intend.

*   **Improves Readability:** Makes the logic clear and unambiguous for anyone reading the code (including your future self!).

**Example:**

```algodraft
$a AS Boolean <- FALSE
$b AS Boolean <- TRUE
$c AS Boolean <- TRUE
$resultDefault AS Boolean
$resultExplicit AS Boolean

// Relying on default precedence (NOT > AND > OR)
$resultDefault <- NOT $a OR $b AND $c
// Evaluation: (NOT FALSE) OR (TRUE AND TRUE)
//           -> TRUE OR TRUE
//           -> TRUE
// $resultDefault becomes TRUE

// Using parentheses to enforce a different evaluation order
$resultExplicit <- (NOT $a OR $b) AND $c
// Evaluation: ((NOT FALSE) OR TRUE) AND TRUE
//           -> (TRUE OR TRUE) AND TRUE
//           -> TRUE AND TRUE
//           -> TRUE
// $resultExplicit becomes TRUE

// Another grouping yields a different result
$resultExplicit <- NOT ($a OR $b AND $c)
// Evaluation: NOT (FALSE OR (TRUE AND TRUE))
//           -> NOT (FALSE OR TRUE)
//           -> NOT (TRUE)
//           -> FALSE
// $resultExplicit becomes FALSE
```

As the last example shows, parentheses can significantly change the outcome and are vital for clarity.

## Purpose of Boolean Expressions

Boolean expressions are rarely used in isolation. Their primary purpose is to control the execution flow in **conditional statements** and **loops**:

*   **`IF` Statements:** Determine whether a block of code should be executed.
    ```algodraft
    IF ($userAge >= 18) AND $hasTicket = TRUE THEN
        NOTIFY {{the user has access}}
    ENDIF
    ```
*   **`WHILE` / `DO-WHILE` Loops:** Determine whether a loop should continue executing.
    ```algodraft
    WHILE $taskIsRunning AND NOT $errorOccurred DO
        // ... perform loop actions ...
    ENDWHILE
    ```

In summary, Boolean expressions are the questions your algorithm asks. They evaluate to `TRUE` or `FALSE` based on literals, variables, comparisons, function results, and logical combinations, allowing your algorithm to make decisions and follow the correct path.
