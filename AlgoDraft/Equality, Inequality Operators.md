---
status: Draft
---
In AlgoDraft, equality and inequality operators are binary operators, used to compare whether two values are the same or different. These comparisons return a **Boolean** result—either `TRUE`, `FALSE`, or, in some cases, a `NotSupportedError` if the operands cannot be meaningfully compared.

### **Available Operators**

| Operator | Meaning                  | Example  | Result |
| -------- | ------------------------ | -------- | ------ |
| `=`      | Equal to                 | `5 = 5`  | `TRUE` |
| `≠`      | Not equal to             | `4 ≠ 7`  | `TRUE` |
| `!=`     | Alternate syntax for `≠` | `4 != 7` | `TRUE` |

> Both `≠` and `!=` are supported for expressing inequality. Use whichever suits your typing environment or personal preference.

**Example**
```
IF $userAnswer ≠ $CORRECT_ANSWER THEN
    NOTIFY {{the answer is wrong, asking for another try}}
ELSE
    NOTIFY {{the answer is correct}}
ENDIF
```

**Notes**
- `=` is **not** an assignment operator. Use <- for assignments in AlgoDraft.

- Always ensure operands are of **compatible types** before comparing them.
    
* Use `≠` for visual clarity in written materials or when teaching, and `!=` when writing code in a text-based editor that doesn't support special symbols.