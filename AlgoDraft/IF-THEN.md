---
status: Draft
---
AlgoDraft provides two ways to execute a snippet of code conditionally based on a single condition being `TRUE`:
* a multi-line structure for complex or multiple actions
* a compact single-line structure for simple, single actions.

# Regular (Multiline) IF-THEN

Execute one or more statements if a condition is `TRUE`. Ideal for readability when multiple actions are needed or when the condition/actions are complex.

**Syntax:**

```
IF <condition> THEN
    <statement_1>
    <statement_2>
    ...
    <statement_N>
ENDIF
```

**Keywords and Clauses:**

- **`IF` keyword:** This keyword signals the beginning of a conditional control structure. It indicates that the following clause will be evaluated to determine the flow of execution.

- **`<condition>` (the `IF` clause):** This is a mandatory expression placed between the `IF` and `THEN` keywords. It must evaluate to a Boolean value (`TRUE` or `FALSE`). The result of this evaluation dictates whether the `THEN` clause will be executed.

- **`THEN` keyword:** This keyword acts as a separator. It concludes the `IF` clause (the condition) and introduces the `THEN` clause (the block of statements to be executed if the condition is `TRUE`).

- **The `THEN` clause (`<statement_1>...<statement_N>`):** This represents the block of one or more AlgoDraft statements appearing after the `THEN` keyword and before the `ENDIF` keywords. This entire sequence of statements is executed sequentially, but only if the `<condition>` (the `IF` clause) evaluates to `TRUE`. For readability, these statements are typically indented.

- **`ENDIF` keyword:** This keyword marks the termination point of the entire multi-line `IF-THEN` structure. Execution control proceeds to the next statement immediately following `ENDIF`, regardless of whether the `THEN` clause was executed or skipped.

**Example:**

```
$score AS Integer <- 85
$level AS String <- "Beginner"
CONST $PASS_THRESHOLD AS Integer <- 70

IF $score >= $PASS_THRESHOLD THEN
    NOTIFY {{the student has passed.}}
    $level <- "Intermediate"
ENDIF

// Execution continues here regardless of the IF outcome
```

# Compact (Single-line) IF-THEN

Execute exactly one simple statement if a condition is `TRUE`. Useful for brevity in straightforward cases.

**Syntax:**

```
IF <condition> THEN <single_statement> ENDIF
```

**Example:**

```
$warnings AS Integer <- 0
$temperature AS Real <- 35.5
CONST $MAX_TEMP AS Real <- 35.0

IF $temperature > $MAX_TEMP THEN $warnings <- $warnings + 1 ENDIF

// Execution continues here
NOTIFY {{processing completed}}
```

**Summary & Usage Guidelines:**

- Use the **multi-line `IF-THEN...ENDIF`** when:
    
    - You need to execute more than one statement conditionally.
    
    - The condition or the statements are long or complex, and breaking them into multiple lines improves readability.
    
- Use the **compact `IF-THEN...ENDIF`** when:
    
    - You need to execute exactly one statement conditionally.
    
    - The condition and the single statement are short and clear enough to fit comfortably and readably on one line.

By offering both, AlgoDraft allows you to choose the structure that best fits the specific situation, balancing conciseness with clarity.