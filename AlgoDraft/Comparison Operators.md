---
status: Planned
---
In AlgoDraft, **comparison operators** are used to compare two values. They are **binary operators**, which means they take **two operands**—one on the left and one on the right—and return a **Boolean value**: either `TRUE` or `FALSE`.

Comparison operations are essential for controlling the flow of programs, especially in decision-making structures like `IF` statements and loops.

| Operator  | Meaning                  |
| --------- | ------------------------ |
| `=`       | Equal to                 |
| `≠`, `!=` | Not equal to             |
| `<`       | Less than                |
| `>`       | Greater than             |
| `≤`, `<=` | Less than or equal to    |
| `≥`, `>=` | Greater than or equal to |

When you use a comparison operator, AlgoDraft evaluates the left and right operands according to the operator's semantics for their data types. The result will always be either `TRUE`, `FALSE`, or a `NotSupportedError` if the comparison is invalid.

```
IF $score ≥ 60 THEN
    NOTIFY {{user passed the exam}}
ELSE
    NOTIFY {{user failed the exam}}
ENDIF
```

In this case, if the value of `$score` is **greater than or equal to** 60, the program notifies that the user passed; otherwise, it notifies refused.
