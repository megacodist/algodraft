---
status: Planned
---
Inherits from IEquatable and adds `<`, `<=`, `>`, `>=`


**Syntax:**

```
INTERFACE IComparable INHERITS IEquatable // If it can be ordered, it can be checked for equality
    OPERATOR this > other -> Boolean ENDOPERATOR
    OPERATOR this < other -> Boolean ENDOPERATOR
    OPERATOR this >= other -> Boolean ENDOPERATOR // Often derived
    OPERATOR this ≥ other -> Boolean ENDOPERATOR
    OPERATOR this <= other -> Boolean ENDOPERATOR // Often derived
    OPERATOR this ≤ other -> Boolean ENDOPERATOR
ENDINTERFACE
```