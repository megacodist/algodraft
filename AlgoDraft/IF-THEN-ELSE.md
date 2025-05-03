---
status: Writing
---
While the `IF-THEN` statement executes a snippet of code only when a condition is `TRUE`, often you need to perform a different set of actions when the condition is `FALSE`. The `IF-THEN-ELSE` statement provides exactly this capability: it defines two mutually exclusive blocks of code, one for the `TRUE` case (the `THEN` clause) and one for the `FALSE` case (the ELSE clause). Only one of these blocks will ever be executed for a single evaluation of the condition.

# Regular (Multiline) IF-THEN-ELSE

**Syntax:**

```
IF <condition> THEN
    <statement_block_A_if_TRUE>
ELSE
    <statement_block_B_if_FALSE>
ENDIF
```

**Keywords and Clauses**

- **`IF` keyword:** Signals the beginning of the conditional control structure.

- **`<condition>` (the `IF` clause):** A mandatory Boolean expression placed between the `IF` and `THEN` keywords. It must evaluate to `TRUE` or `FALSE`. The result determines which clause (`THEN` or `ELSE`) `will` execute.

- **`THEN` keyword:** Separates the `IF` clause (condition) from the first block of statements (the `THEN` clause).

- **The `THEN` clause (`<statement_block_A_if_TRUE>`):** This block contains one or more AlgoDraft statements located between the `THEN` keyword and the `ELSE` keyword. This entire block is executed only if the `<condition>` evaluates to `TRUE`. These statements are typically indented.

- **`ELSE` keyword:** This keyword introduces the alternative block of statements. It signifies the start of the code to be executed if the initial `<condition>` evaluates to `FALSE`.

- **The `ELSE` clause (`<statement_block_B_if_FALSE>`):** This block contains one or more AlgoDraft statements located between the `ELSE` keyword and the `ENDIF` keyword. This entire block is executed only if the `<condition>` evaluates to `FALSE`. These statements are also typically indented.

- **`ENDIF` keyword:** This keyword marks the termination point of the entire `IF-THEN-ELSE` structure. Execution always proceeds to the statement immediately following `ENDIF` after one of the two clauses (`THEN` or `ELSE`) has finished executing.

# Compact (Single-line) IF-THEN-ELSE

Similar to how `IF-THEN` has a compact form for single actions, AlgoDraft also provides a compact `IF-THEN-ELSE` structure. This is useful when you need to execute one of two possible single statements based on a condition, and the condition and statements are short enough to be clearly expressed on a single line.

**Syntax:**

```
IF <condition> THEN <single_statement_A> ELSE <single_statement_B> ENDIF
```

**Notes**:

- **Constraint:** You must have **exactly one** statement for the `THEN` clause and **exactly one** statement for the `ELSE` clause.

- Use the compact form when the condition and both outcome statements are **short and simple**.

- It's ideal for quick assignments based on a condition (e.g., setting a status, incrementing one of two counters).

- **Avoid** using it if the condition is complex, or if either the `TRUE` or `FALSE` case requires more than one action. Use the standard multi-line `IF-THEN-ELSE` structure for clarity in those scenarios.

Example **1**:

```
$age AS Integer
$canDrink AS Boolean <- FALSE
CONST $LEGAL_AGE AS Integer <- 21

$age <- INPUT {{user's age}}
IF $age >= $LEGAL_AGE THEN $canDrink <- TRUE ELSE $canDrink <- FALSE ENDIF

NOTIFY {{drinking allowance based on $canDrink flag}}
```

# Conditional Expression

AlgoDraft provides not only conditional statements (like `IF-THEN` and `IF-THEN-ELSE`) to control the flow of execution but also a powerful conditional expression. This construct allows you to choose between two different **values** based on a condition. The entire expression evaluates to a single resulting value, making it very useful for concisely determining a value to be used in assignments, function calls, or calculations. **Remember, this is an expression, not a statement.** It produces a value.

**Syntax:**

```
<value_if_TRUE> IF <condition> ELSE <value_if_FALSE> ENDIF
```

- **`<value_if_TRUE>`**: This is the expression (e.g., a literal, a variable, a calculation) whose value will be the result of the entire conditional expression if the `<condition>` evaluates to `TRUE`.

- **`IF` keyword**: Separates the `TRUE`-case value from the condition.

- **`<condition>`**: A mandatory Boolean expression that must evaluate to `TRUE` or `FALSE`.

- **`ELSE` keyword**: Separates the condition from the `FALSE`-case value.

- **`<value_if_FALSE>`**: This is the expression whose value will be the result of the entire conditional expression if the `<condition>` evaluates to `FALSE`.

- **`ENDIF` keyword**: The single keyword that terminates the conditional expression structure.

**Notes**:

- **It's an Expression:** This cannot be stressed enough. Unlike `IF-THEN[-ELSE]` statements which direct which code runs, this expression produces a value.

- **Context:** Use it anywhere you need a value:
    
    - Right-hand side of an assignment: `$var <- ... IF ... ELSE ... ENDIF`
    
    - As an argument in a function call: `DoSomething( ... IF ... ELSE ... ENDIF )`
    
    - Within larger calculations: `$total <- base + ( ... IF ... ELSE ... ENDIF ) * factor`

- **Contrast with Statements:** `IF-THEN...ENDIF` and `IF-THEN-ELSE...ENDIF` are statements; they can contain multiple lines of code, perform actions like `NOTIFY`, and don't directly produce a value themselves (though they might assign one). This conditional expression only produces a value.

- **Avoid Side Effects:** The `<value_if_TRUE>` and `<value_if_FALSE>` components should ideally be simple values or calculations. Avoid placing statements with side-effects (like assignments or `NOTIFY`) within them, as this can be confusing and is not the intended use. Use `IF-THEN-ELSE` statements for conditional actions.

- **Readability:** This expression is excellent for conciseness when the condition and values are simple. If the logic becomes complex, a multi-line `IF-THEN-ELSE` statement is often clearer.

**Example**:

```
CONST $MAJORITY_AGE AS Integer <- 18

$userAge AS Integer <- INPUT {{user's age}}
$status AS String <- "Adult" IF $userAge >= $MAJORITY_AGE ELSE "Minor" ENDIF

NOTIFY {{user status ($status)}}
```