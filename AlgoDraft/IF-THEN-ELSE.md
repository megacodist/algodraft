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

- **`<condition>` (the `IF` clause):** A mandatory Boolean expression placed between the `IF` and `THEN` keywords. It must evaluate to `TRUE` or `FALSE`. The result determines which clause (`THEN` or ELSE) `will` execute.

- **THEN keyword:** Separates the IF clause (condition) from the first block of statements (the THEN clause).
    
- **The THEN clause (<statement_block_A_if_TRUE>):** This block contains one or more AlgoDraft statements located between the THEN keyword and the ELSE keyword. This entire block is executed only if the <condition> evaluates to TRUE. These statements are typically indented.
    
- **ELSE keyword:** This keyword introduces the alternative block of statements. It signifies the start of the code to be executed if the initial <condition> evaluates to FALSE.
    
- **The ELSE clause (<statement_block_B_if_FALSE>):** This block contains one or more AlgoDraft statements located between the ELSE keyword and the END IF keywords. This entire block is executed only if the <condition> evaluates to FALSE. These statements are also typically indented.
    
- **END IF keyword:** This two-word keyword pair marks the termination point of the entire IF-THEN-ELSE structure. Execution always proceeds to the statement immediately following END IF after one of the two clauses (THEN or ELSE) has finished executing.
