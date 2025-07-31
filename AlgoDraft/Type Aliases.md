---
status: Planned
---
- **What is Type Aliasing?**
    
    - Type aliasing in AlgoDraft allows you to give a new, often more descriptive or simpler name to an already existing type expression.
    
    - It's important to understand that an alias **does not** create a new, distinct type; it simply provides an alternative name for an existing one. The alias can be used interchangeably with the original type it refers to.

- **Why Use Type Aliases in AlgoDraft?**
    
    - **Enhanced Readability:** Complex type declarations (like nested Lists) can be made much easier to understand at a glance by using a shorter, more meaningful alias. For example, `PixelRow` is clearer than `List<Pixel>`.
    
    - **Improved Abstraction:** You can hide the underlying structural details of a type behind a more conceptual name. This allows you to focus on what the type represents rather than how it's constructed. For instance, using `SparseMatrix` lets you think at a higher level than its specific List of `RECORD`s implementation.
    
    - **Increased Maintainability:** If the underlying structure of a type needs to change, you ideally only need to update the `ALIAS` definition in one place. All parts of your algorithm that use the alias will then automatically refer to the new structure, simplifying updates.
    
    - **Domain-Specific Modeling:** Type aliases help you create type names that reflect the vocabulary of the problem domain you are working in. This makes your algorithm more self-documenting and easier for others (and your future self) to understand (e.g., using `UserID` instead of just `Integer`).

**Syntax**:

```
ALIAS
    NewTypeName  // The custom, meaningful name you are defining
    FOR
    ExistingTypeExpression // The AlgoDraft type being given a new name
[ENDALIAS] // ENDALIAS is optional if the ALIAS definition fits on a single line
```

**Breakdown**:

- `ALIAS` / `ENDALIAS`: These keywords delimit the alias definition block. `ENDALIAS` can be omitted if the entire definition is on a single line.

- `NewTypeName`: This is the identifier (name) for your new type alias. It should follow AlgoDraft's standard naming conventions.

- `FOR`: This keyword connects the `NewTypeName` to the `ExistingTypeExpression`.

- `ExistingTypeExpression`: This is the crucial part and can be:
    
    - A built-in basic type (e.g., `Integer`, `Boolean`).
    
    - A built-in data structure (e.g., `List<Float>`, `Map<String TO Boolean>`).
    
    - A user-defined types like `RECORD` and `CLASS`.
    
    - A function signature type.
    
    - Another already defined type alias.
    
    - An imported type name (from `IMPORT ... FROM ...`).
    
    - A conceptually declared type name (from `DECLARE ... AS {{NLD}}`).

# Aliasing Basic and Built-in Collection Types

You can create aliases for basic types to give them more semantic meaning within your algorithm, or to simplify complex collection type expressions.

```
// For domain modeling with basic types
ALIAS UserIdentifier FOR String ENDALIAS
ALIAS ProductPrice FOR Float ENDALIAS
ALIAS MaxRetries FOR Integer ENDALIAS

// For simplifying collection types
ALIAS NameList FOR List<String> ENDALIAS
ALIAS FeatureFlags FOR Map<String TO Boolean> ENDALIAS
ALIAS Coordinate FOR Tuple<Float, Float>
ALIAS AdjacencyMatrix FOR List<List<Integer>> ENDALIAS
```

And you can use them to declare variables:

```
currentUser AS UserIdentifier
itemPrice AS ProductPrice
activeUsers AS NameList
appSettings AS FeatureFlags
startPoint AS Coordinate
```

# Aliasing User-Defined Record Types

If you define custom `RECORD` structures, you can create aliases for them, perhaps to provide a more general or alternative conceptual name.

```
RECORD EmployeeDetails
    employeeId AS String,
    department AS String,
    yearsOfService AS Integer
ENDRECORD

// Alias for the EmployeeDetails record
ALIAS
    StaffMemberRecord
    FOR
    EmployeeDetails
ENDALIAS

// Another record definition
RECORD ColorRGB
    red AS Integer,    // Assuming values 0-255
    green AS Integer,
    blue AS Integer
ENDRECORD

ALIAS Pixel FOR ColorRGB ENDALIAS // Pixel is a conceptual name for ColorRGB
```

Defining variables:

```
newHire AS StaffMemberRecord
backgroundColor AS Pixel
```

# Aliasing Function Signatures (Function Types)

This is a particularly powerful use of `ALIAS`. It allows you to define a name for a specific function "shape" (its parameter types, modes, and return type). This is very useful when working with higher-order functions (functions that take other functions as arguments or return them) or for defining types for callbacks or event handlers.

**Syntax:**

```
ALIAS AliasName FOR Function<...> ENDALIAS
```

The syntax for a function signature type within the `FOR` clause mirrors the signature part of a function definition, but typically without parameter names and their possible default values.

```
// Recall: Signature Type Syntax for ALIAS typically looks like:
// FUNCTION ([PARAM_MODE] Type1, [PARAM_MODE] Type2, ...) [-> ReturnType]

// Example: An alias for a function that checks if an integer meets a condition
ALIAS
    IntegerPredicate
    FOR
    Function<Integer -> Boolean>
ENDALIAS

// Example: An alias for a procedure that modifies a list of strings
ALIAS
    StringListModifier
    FOR
    Function<INOUT List<String> -> NULL> // No return type specified (procedure)
ENDALIAS

// Example: A more complex alias for a transformation function
ALIAS
    DataTransformer
    FOR
    Function<(RawData, OUT ProcessedData, ConfigOptions) -> Boolean>
    // Assuming RawData, ProcessedData, ConfigOptions are other defined types or aliases
ENDALIAS
```

Usage in Parameter Lists:

```
FUNCTION FilterNumbers(
		IN numbers AS List<Integer>,
		IN test AS IntegerPredicate,
		) -> List<Integer> :=
    filteredList AS List<Integer> <- NEW List<Integer>
    FOR EACH num IN numbers DO
        IF test(num) THEN // Call the predicate function
            DO {{Append @num to @filteredList}}
        ENDIF
    ENDFOR
    RETURN filteredList
ENDFUNCTION

processor AS StringListModifier // Variable to hold a function of this type
```

**A.3.6. Aliasing Other Aliases (Chaining Aliases)**

- You can define an alias based on another existing alias. This allows for building layers of abstraction or specialization.
    
    ```
    ALIAS Identifier FOR String ENDALIAS
    ALIAS UserID FOR Identifier ENDALIAS       // UserID is an Identifier, which is a String
    ALIAS ProductSKU FOR Identifier ENDALIAS   // ProductSKU is also an Identifier
    ```
    
    content_copydownload
    
    Use code [with caution](https://support.google.com/legal/answer/13505487).Pseudocode
    
- While this is possible, be mindful that excessive chaining (e.g., AliasC FOR AliasB, AliasB FOR AliasA) can sometimes make it harder to quickly understand the ultimate base type. Use it judiciously for clarity.
    

**A.3.7. Using Type Aliases in Declarations and Signatures**

- Once defined, a type alias can be used almost anywhere you would use a regular type name:
    
    - **Variable declarations:** myVariable AS MyCustomAlias
        
    - **Function parameter types:** FUNCTION ProcessData(IN data AS MyCustomAlias) ...
        
    - **Function return types:** FUNCTION CreateObject() -> MyCustomAlias ...
        
    - **Field types within RECORD definitions:** myField AS MyCustomAlias
        
    - **Element types in collection type expressions:** List<MyCustomAlias>, Map<KeyType TO MyCustomAlias>
        
    - In the FOR clause of another ALIAS definition.
        

**A.3.8. Scope of Type Aliases**

- (If AlgoDraft defines specific scoping rules, describe them here. For most pseudocode, type aliases are typically considered to have a broad scope, often global within the algorithm's definition, unless explicitly stated otherwise, like being defined within a specific module if your pseudocode supports modules). For simplicity in AlgoDraft, we can assume aliases are visible from their point of definition onwards within the current scope (e.g., the entire algorithm or module).
    

---

This revised version adheres to your specified AlgoDraft syntax constraints and provides a comprehensive guide to using type aliases.






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
