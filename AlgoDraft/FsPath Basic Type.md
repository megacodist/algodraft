---
status: Writing
---
Represents a class to instantiate objects to work with file system items.

# Operations

## Existence

This unary postfix operator checks the existence of this `FsPath` object:

**Syntax**:

```AlgoDraft
OPERATOR (this EXISTED) -> Boolean ENDOPERATOR
OPERATOR (this NOT EXISTED) -> Boolean ENDOPERATOR
```

## Getting Folder

This unary prefix operator gets the folder of this `FsPath` object:

```AlgoDraft
OPERATOR (FOLDER OF this) -> FsPath ENDOPERATOR
```
