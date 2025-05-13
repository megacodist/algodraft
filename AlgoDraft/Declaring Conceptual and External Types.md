---
status: Draft
---
While AlgoDraft provides built-in types and allows you to create aliases for complex structures, often when designing algorithms, you need to refer to types whose precise structure is:

*   Defined by an **external library or system** (e.g., a database connection, a file handle, a UI widget).

*   **Abstract or conceptual** at the current stage of design, with details to be filled in later.

*   Intentionally kept **opaque** to make the algorithm more generic or library-agnostic.

For these scenarios, AlgoDraft provides a special use of the `DECLARE` keyword to introduce a type name based on a **Natural Language Description (NLD)**. This NLD serves as the "definition" of the type's meaning and intended use within your AlgoDraft pseudocode.

**Purpose of Declaring Conceptual/External Types:**

*   **Library Agnosticism:** Allows you to design algorithms that interact with external systems without tying your pseudocode to the specific type names of a particular library.

*   **Abstraction:** You can refer to complex external entities by a simple conceptual name (e.g., `WebResponse` instead of the intricate object structure a specific HTTP library might return).

*   **Design Focus:** Enables you to focus on the algorithmic logic involving these types without getting bogged down in their low-level implementation details. The NLD captures the necessary understanding.

*   **Opaque Types:** These declared types are treated as "opaque" by AlgoDraft – their internal structure isn't defined within the pseudocode itself. Operations on them are typically assumed based on the NLD or performed by functions that also understand these external types (perhaps functions imported from an external source or other NLD-described functions).

**Syntax:**

```AlgoDrat
DECLARE TypeName AS {{Natural Language Description of the type}} [ENDDECLARE]
```

*   **`DECLARE`**: The keyword used to introduce this conceptual type name into the current scope.

*   **`TypeName`**: The identifier you will use in AlgoDraft to refer to this conceptual or external type.

*   **`AS`**: The keyword connecting the type name to its description.

*   **`{{Natural Language Description}}`**: A clear, concise explanation written in plain language (enclosed in double curly braces `{{ }}`) that describes what the `TypeName` represents, its origin (if relevant, e.g., "from the OS file system"), or its expected behavior. This NLD is the primary source of meaning for this type within the AlgoDraft context.

*   **`[ENDDECLARE]`**: Optional terminator, useful if the NLD spans multiple lines or for explicit closure. Can be omitted if the statement fits on a single line.

**How it Works:**

When you use this syntax, `TypeName` becomes a recognized type identifier within your AlgoDraft code. You can then use it in:

*   Variable declarations (`$myVar AS TypeName`).

*   Function parameter type declarations (`IN $param AS TypeName`).

*   Function return type declarations (`-> TypeName`).

*   In `ALIAS` definitions (`ALIAS MyHandle FOR TypeName`).

AlgoDraft itself doesn't "understand" the internal structure of `TypeName` beyond what's implied by its usage and the NLD. The validity of operations performed on variables of this type is determined by the logic of your algorithm and the assumed capabilities described by the NLD or by the functions that operate on these types.

**Examples 1: Interfacing with an Operating System:**

```AlgoDraft
DECLARE
	FileHandle
	AS
	{{an opaque handle provided by the operating system to an open file}}
ENDDECLARE
DECLARE
	ProcessID
	AS
	{{a unique identifier for a running operating system process}}
ENDDECLARE

FUNCTION ReadFromFile(
		IN $handle AS FileHandle,
		OUT $buffer AS String,
	) -> Boolean :=
	// Implementation details depend on the conceptual OS API
	TRY
		$buffer <- DO {{do some system operation with $handle}}
		RETURN TRUE
	CATCH
		RETURN FALSE
	ENDTRY
ENDFUNCTION

$logFile AS FileHandle <- OpenFile("system.log") // OpenFile conceptually returns a FileHandle
```

**Example 2: Working with a Web API Response:**

```AlgoDraft
// Getting JSON object from a JSON HTTP Response...
// Implementation details depend on the concrete HTTP and JSON libraries
DECLARE
	HttpResponse
	AS
	{{the entire response object from an HTTP request, potentially including
		headers, status, and body}}
ENDDECLARE
DECLARE JsonObject AS {{a structured object representing parsed JSON data}}

FUNCTION GetJsonBody(IN $response AS HttpResponse) -> JsonObject :=
	$rawBody AS String <- DO {{get the body, of course as text, from $response}}
	$json AS JsonObject <- DO {{parse $rawBody as text into JSON object}}
	RETURN $json
ENDFUNCTION
```

**Example 3: Library-Agnostic Graphics:**

```AlgoDraft
// The following code 

DECLARE
    MdRoot
    FOR
    {{the root element in the markdown AST hierarchy}}
ENDDECLARE
DECLARE
    MdElement
    FOR
    {{the base element of all elements in the Markdown AST hierarchy}}
ENDDECLARE

$mdFile AS File<String> <- DO {{open the Markdown document in text, read mode}}
$ast as MdRoot <- DO {{parse $mdFile into Markdown AST}}
FOR EACH ($elem AS MdElement) IN $ast.$children DO
    // Do something with $elem...
ENDFOR
```

4.  **Database Interaction:**
    ```pseudocode
    DECLARE DatabaseConnection AS {{an active connection to a relational database}} ENDDECLARE
    DECLARE QueryResult AS {{the set of rows returned from a database query}} ENDDECLARE

    FUNCTION ExecuteQuery(IN $conn AS DatabaseConnection, IN $sqlQuery AS String) -> QueryResult
    :=
        {{Implementation sends query to database via connection and returns results}}
    ENDFUNCTION
    ```

**Using Declared Conceptual Types with `ALIAS`:**

You can also create aliases for these conceptually declared types if needed for further abstraction or naming convenience:

```pseudocode
DECLARE NetworkSocket AS {{a communication endpoint for network traffic}} ENDDECLARE
ALIAS
    ClientSocket
    FOR
    NetworkSocket // ClientSocket is now conceptually a NetworkSocket
ENDALIAS
```

**Summary:**

The `DECLARE TypeName AS {{NLD}}` construct is a powerful tool in AlgoDraft for designing algorithms that interact with external or abstract systems. It allows you to:

*   Introduce necessary type names without defining their full internal structure.
*   Rely on Natural Language Descriptions to convey their meaning and intended use.
*   Keep your pseudocode focused on the algorithm's logic, promoting clarity and library independence.

This mechanism is particularly useful during the early stages of design or when aiming for a high-level, abstract representation of an algorithm.