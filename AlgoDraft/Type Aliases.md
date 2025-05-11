---
status: Planned
---
As your AlgoDraft programs grow, you might find yourself repeatedly using complex type definitions (like a specific kind of `List` or a `Function`) or wanting to give more semantically meaningful names to basic types within a certain context. AlgoDraft provides **Type Aliases** for this purpose.

A type alias allows you to define a **new name (an alias)** for an **existing type expression**. This new name can then be used anywhere you would normally use the original type name (e.g., in variable declarations, parameter types, return types). Type aliases do not create new, distinct types; they simply provide an alternative name for an existing one.

**Benefits of Using Type Aliases:**

- **Readability:** Makes code easier to understand by using names that better reflect the purpose of the data (e.g., `UserID` is more descriptive than just `String` or `Integer` for an `ID`).

- **Maintainability:** If a complex type definition used in many places needs to change, you only need to update it in the one place where the alias is defined.

- **Simplicity:** Can simplify long or complex type expressions by giving them a shorter, more manageable name.

AlgoDraft offers two ways to define type aliases, depending on the complexity of the type expression you are aliasing:

 **Single-Line Alias (for simpler type expressions):**

This form is ideal when the existing type expression is concise.

```
ALIAS NewTypeName FOR <type_expr> [ENDALIAS]
```

- **`ALIAS`**: The keyword that starts the type alias definition.

- **`NewTypeName`**: The new, custom name you are creating for the type. This should follow PascalCase convention.

- **`FOR`**: This keyword in this context links the new name to the existing type.

- **`<type_expr>`**: The type you are creating an alias for (e.g., `Integer`, `List<String>`, `Boolean`, another alias).

- **`ENDALIAS`**: The optional `ENDALIAS` to signify the end of alias definition.

**Examples (Single-Line):**

```
// Aliasing basic types for semantic clarity
ALIAS UserID FOR Integer
ALIAS EmailAddress FOR String
ALIAS IsCompleteFlag FOR Boolean
ALIAS PixelValue FOR Integer

// Aliasing a parameterized data structure
ALIAS NameList FOR List<String>
ALIAS UserScores FOR Mapping<UserID TO Integer> // UserID is itself an alias

// Define an alias for a function that takes two integers and returns a boolean
ALIAS IntegerPredicate FOR FUNCTION (IN Integer, IN Integer) -> Boolean
```

**Multi-Line Alias (for complex type expressions):**

When the `<type_expr>` is long or structured (like a `FUNCTION` signature), the multi-line format provides better readability.

```
ALIAS
    NewTypeName
	FOR
    <type_expr> // Can span multiple lines
ENDALIAS
```

- **`ALIAS ... ENDALIAS`**: These keywords delimit the multi-line alias definition.

- **`NewTypeName`**: The new name, on its own line.

- **`FOR`**: Keyword, on its own line.

- **`<type_expr>`**: The detailed type definition, which can be formatted over multiple lines for clarity.

**Examples (Multi-Line):**

```
// Aliasing a function signature
ALIAS
    MyFunctionTypeAlias // Name of the new function type
    FOR
    FUNCTION ( // No name, just the signature structure
        IN TypeA,         // Parameter modes and types are part of the signature type
        OUT TypeB,
        INOUT TypeC
	    ) -> ReturnTypeD     // Optional return type
ENDALIAS
```
