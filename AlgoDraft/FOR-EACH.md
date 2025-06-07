---
status: Planned
---
**Syntax:**

```
FOR EACH item IN source DO
	DO {{Do something with @item}}
ENDFOR
```

**Semantics:**

The `FOR EACH` works as follows:

- The `source` expression is evaluated.

- **Type Check on source:**
    
    1. **If `source IS AN IIterable`:** An iterator is obtained by calling `iter <- ITERATOR OF source`. The loop then proceeds to use this newly obtained Iterator.
    
    2. **Else if `source IS AN Iterator`:** The loop continues consuming the provided source `iter <- source`.
    
    3. **Else (if source is neither `IIterable` nor `Iterator`):** An error is raised, as the source cannot be iterated.

- **Iteration Process (using the determined Iterator):**
    
    - At the start of every iteration, the loop checks if there is any value available by `IF iter EXHAUSTED`.
    
    - If no other value remains, the loop terminates.
    
    - If there is a value, loop obtains it by `item <- NEXT iter`.