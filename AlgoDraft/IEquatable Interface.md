---
status: Planned
---
Implements equality and inequality

**Syntax:**

```
INTERFACE IEquatable
    OPERATOR this = other -> Boolean ENDOPERATOR
    OPERATOR this ≠ other -> Boolean ENDOPERATOR   // Often derived from =
    OPERATOR this != other -> Boolean ENDOPERATOR
ENDINTERFACE
```