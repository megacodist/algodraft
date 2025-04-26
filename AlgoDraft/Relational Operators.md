---
status: Writing
---
Relational operators in AlgoDraft are used to compare the relative size or order of two values. They determine whether one operand is less than, greater than, or equal to another. The result is always a **Boolean** value—`TRUE`, `FALSE`, or a `NotSupportedError` if the operands cannot be compared.

### **Available Operators**

| Operator  | Meaning                  | Example           | Result |
| --------- | ------------------------ | ----------------- | ------ |
| `<`       | Less than                | `3 < 5`           | `TRUE` |
| `>`       | Greater than             | `7 > 2`           | `TRUE` |
| `≤`, `<=` | Less than or equal to    | `4 ≤ 4`, `4 <= 4` | `TRUE` |
| `≥`, `>=` | Greater than or equal to | `6 ≥ 3`, `6 >= 3` | `TRUE` |

> AlgoDraft supports both symbolic (`≤`, `≥`) and ASCII-friendly (`<=`, `>=`) versions for convenience. They behave exactly the same.

**Example**
In this example, if the `temperature` is above the "hotness threshold", the program notifies **It is hot**; otherwise, it notifies **It is not hot**.
```
IF $temporature > $THRESHOLD THEN
    NOTIFY {{the weather is hot}}
ELSE
    NOTIFY {{the weather is not hot}}
ENDIF
```

**Notes**
- Remember that `<=` and `≥` (or `>=`) check for **equality as well as order**. For example, `5 <= 5` returns `true`.
    
- Always verify that the types you compare make sense together to avoid errors.

*  When typing is limited (like in plain text editors), use `<=` and `>=`. When writing documentation or tutorials, the symbols `≤` and `≥` can make expressions easier to read.