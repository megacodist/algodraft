---
status: Writing
---
Strings are 0-based-indexed sequences of characters.

# Operations

## Concatenation

**Syntax**:

```AlgoDraft
OPERATOR this + (str AS String) -> String;
OPERATOR this + (char AS Char) -> String;
OPERATOR (char AS Char) + this -> String;
```

**Semantics**:

Prepends the character or append the provided character/string to this string as a new string.