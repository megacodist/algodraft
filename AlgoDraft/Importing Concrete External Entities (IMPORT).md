---
status: Draft
---
The `IMPORT` block is used when you want to reference external entities that are understood to come from more specifically identified external sources (like a library, module, or API). It bridges your AlgoDraft design closer to potential implementation details by acknowledging a defined origin.

**Purpose and Philosophy:**

- **Focus:** Making entities from known external sources available within AlgoDraft.

- **Mechanism:** You declare an AlgoDraft name (for a type, constant, or function) and specify its `FROM SourceIdentifier`. You can also provide a local alias using `AS LocalName`.

**Syntax**:

```
IMPORT
    // Import statements for types, constants, and functions using 'FROM SourceIdentifier'
ENDIMPORT
```

Within this block, each declaration specifies the kind of entity, its name(s) in the external source, its AlgoDraft type (for constants) or optional signature (for functions), the source it comes from, and an optional local alias.

# importing External Types

Imported types are generally treated as opaque in AlgoDraft unless further mechanisms (beyond the scope of this basic import) define their structure.

**Syntax (Single-line, potentially multiple types):**

```
TYPE ExtName1 [AS Local1], ExtName2 [AS Local2], ... FROM SourceIdentifier [ENDTYPE]
```

The `ENDTYPE` is optional if the entire declaration is on a single line.

**Syntax (Block form, typically for one or more types from the same source):**

```
TYPE
    ExternalTypeName1 [AS LocalName1],
    ExternalTypeName2 [AS LocalName2],
    ...
    FROM SourceIdentifier
ENDTYPE
```

**Example**:

```
IMPORT
    TYPE FileStream, DirectoryInfo AS DirInfo FROM System.IO.Abstractions

    TYPE
        HttpRequest, HttpResponse
        FROM Network.HTTP.Client
    ENDTYPE

    TYPE XmlDocument AS Dom
        FROM XML.Parser.Core
    ENDTYPE
ENDIMPORT
```

# Importing External Constants

Imported constants must be given an AlgoDraft type. Their value is sourced externally.

**Syntax (Single-line, potentially multiple constants):**

```
CONST $EXT_CONST_1 AS T1 [AS $LOCAL_1], $EXT_CONST_2 AS T2 [AS $LOCAL_2], ... FROM Source [ENDCONST] 
```

The `ENDCONST` is optional for single-line declarations.

**Syntax (Block form, for one or more constants from the same source):**

```
CONST
    $EXT_CONST_1 AS T1 [AS $LOCAL_1],
    $EXT_CONST_2 AS T2 [AS $LOCAL_2],
    ...
    FROM SourceIdentifier
ENDCONST
```

**Example**:

```
IMPORT
    CONST $MAX_PATH AS Integer FROM OS.Limits, $EOL AS String FROM OS.Text ENDCONST

    CONST
        $DEFAULT_TIMEOUT_SECONDS AS Integer AS $NetworkTimeout,
        $MAX_RETRIES AS Integer
        FROM Network.Configuration
    ENDCONST
ENDIMPORT
```

# Importing External Functions

AlgoDraft provides flexibility when importing functions: you can import just by name (trusting the external definition) or provide an AlgoDraft-compatible signature for clarity.

## Importing by Name Only

**Syntax (Single-line, potentially multiple function names):**

```
FUNCTION ExtFuncName1 [AS Loc1], ExtFuncName2 [AS Loc2], ... FROM Source [ENDFUNCTION]
```

The `ENDFUNCTION` is optional for single-line declarations.

**Syntax (Block form, for one or more function names from the same source):**

```
FUNCTION
    ExternalFunctionName1 [AS LocalName1],
    ExternalFunctionName2 [AS LocalName2],
    ...
    FROM SourceIdentifier
ENDFUNCTION
```

- **Explanation:** Only the function name(s) (and optional local aliases) are brought into scope. The full signature (parameters, types, modes, return value) is assumed to be defined and enforced by the external source. AlgoDraft performs no signature checking itself for these functions; responsibility for correct usage (number/types of arguments, handling returns) lies with the algorithm designer based on their knowledge of the external library.

- **Use Case:** Convenient for well-known external APIs where re-specifying the signature in AlgoDraft is redundant or for rapid prototyping.

**Example**:

```
IMPORT
    FUNCTION ReadAllText, WriteAllText AS SaveText FROM File.Utilities

    FUNCTION
        OpenConnection,
        CloseConnection,
        SendData AS SendNetData
        FROM SimpleNetworkLibrary
    ENDFUNCTION
ENDIMPORT
```

## Importing with an AlgoDraft-Compatible Signature

**Syntax (Block form is typical for clarity):**

```
FUNCTION AlgoDraftFunctionSignature // Full signature as defined in AlgoDraft
    FROM SourceIdentifier [AS LocalFunctionName]
ENDFUNCTION
```

- **Explanation:** You provide a complete AlgoDraft function signature (name, parameters with directionality and types, and return type). This explicitly tells AlgoDraft how the external function should be treated within your pseudocode in terms of its interface. While AlgoDraft doesn't verify this signature against the actual external library, it guides the AlgoDraft user on how to call it and what to expect, enabling more explicit design. The function name in the AlgoDraft signature is the name used to call it (unless aliased with AS).

- **Use Case:** When you want to be explicit within your AlgoDraft design about the expected interface of an external function, promoting clarity and a more defined contract for its usage. This is also useful if the external function's actual name is different or if you want to present a slightly adapted view of its parameters for AlgoDraft purposes.

**Example**:

```
IMPORT
    FUNCTION GetDatabaseRecord (
	        IN $queryId AS String,
	        OUT $recordData AS UserRecord // UserRecord is an AlgoDraft or CONCEPT type
	    ) -> Boolean // TRUE if record found
        FROM Database.AccessLayer.Queries
        AS FetchUserRecordByID
    ENDFUNCTION
ENDIMPORT
```

For imported functions, providing the full AlgoDraft-compatible signature is generally recommended when clarity, a more defined contract within AlgoDraft, and explicit design are paramount. Importing by name only offers a shorthand for well-understood or simple external APIs where detailed signature repetition is deemed unnecessary by the designer.