---
status: Draft
---
# The Club of Conceptual Buddies: An Introduction to `CONCEPT` Blocks

In AlgoDraft, algorithms often need to interact with or refer to ideas, data structures, functionalities, or constant values whose precise, low-level implementation details are either unknown at the early design stage, intentionally kept abstract, external to the core AlgoDraft logic, or perhaps even beyond AlgoDraft's direct expressive capabilities for that specific detail.

The **`CONCEPT ... ENDCONCEPT`** block is AlgoDraft's dedicated mechanism for declaring these **conceptual entities**. It allows you to give these abstract notions a name and a clear, human-understandable specification within your algorithm design.

## The Role of Natural Language Descriptions (NLDs) within `CONCEPT`

Every entity declared within a `CONCEPT` block is defined using the `:= {{description}}` syntax. The Natural Language Description (NLD), enclosed in `{{ }}`, provides the complete conceptual definition for AlgoDraft's purposes. It describes the entity's essential nature, its expected behavior or contract, its origin (if conceptualized as external), or the minimal interface it's expected to present (especially for conceptual records and classes).

## Message to Implementers/Designers & Realization Paths

Entities declared in `CONCEPT` blocks are abstract placeholders that signal a requirement. They await "realization" as the design progresses towards a concrete implementation. This signals to anyone reading or implementing the AlgoDraft design:

> Dear implementer/future designer, the following entity represents a need within this algorithm. AlgoDraft, by design, might not express the full depth of this external system or complex type, or I (the designer) am choosing to keep it abstract for now. When moving towards concrete implementation, this concept needs to be addressed. This might involve one of three primary realization paths:
>
> 1. **Implementation in AlgoDraft:** The entity (e.g., a `CONCEPT FUNCTION`) is fully implemented using standard AlgoDraft statements and structures. The `CONCEPT` declaration serves as its initial specification and contract.
>
>2. **Integration with External Libraries/APIs/Systems:** The entity corresponds to functionality or data provided by an external software library, operating system feature, hardware interface, or remote service. This often involves finding and using appropriate APIs from that external source. (This path might later be documented in AlgoDraft using `IMPORT` blocks if the design is refined to that level of detail).
>
>3. **Leveraging Target Programming Language Capabilities:** The chosen target programming language (for final implementation beyond AlgoDraft) may offer built-in features, types, or paradigms that directly or effectively realize the conceptual entity.
>
> Your task is to choose or combine these paths appropriately to fulfill the conceptual requirement based on project needs and available technologies.

## Benefits of Using CONCEPT Blocks

- **Design-Time Abstraction:** Focus on what an entity is or does, deferring the how.

- **OS/Library/Platform Agnosticism:** Design core logic independently of specific external implementations. "Program to interface NOT implementation."

- **Clear Documentation:** Explicitly documents abstract dependencies, assumptions, and areas requiring further realization.

- **Highlights Design Decisions:** Makes it clear which parts are abstract versus concretely defined in AlgoDraft.

## Syntax

All conceptual declarations are grouped within a `CONCEPT ... ENDCONCEPT` block. You can have multiple `CONCEPT` blocks in your AlgoDraft document, perhaps to organize different sets of conceptual entities.

**General Structure:**

```
CONCEPT
    // One or more declarations of conceptual types, constants, or functions.
    // Each declaration uses the ':= {{NLD}}' pattern.
ENDCONCEPT
```

Each entity declaration within the block specifies its kind (`TYPE`, `RECORD`, `CLASS`, `CONST`, `FUNCTION`), its AlgoDraft name (and type/signature where applicable), followed by `:=` and its defining NLD.

# Declaring Conceptual External Types

Conceptual types allow you to name and refer to data structures or kinds of data whose detailed internal structure is not being defined within AlgoDraft at this point.

## Generic Conceptual Types (`TYPE`)

Use `TYPE` for conceptual types where you are not specifying a record-like or class-like nature, or when it's truly opaque.

**Syntax (Single-line):**

```
TYPE TypeName := {{NLD describing the type's concept}} [ENDTYPE]
```

The `ENDTYPE` keyword is optional if the entire declaration is on a single line.

**Syntax (Multi-line NLD):**

```
TYPE TypeName :=
    {{A detailed Natural Language Description, potentially spanning multiple lines,
	  explaining what this type represents, its purpose, and any key
	  conceptual properties or constraints known at this design stage.}}
ENDTYPE
```

`TypeName` becomes known to AlgoDraft. It's treated as an opaque type; its characteristics, any implied structure, and valid operations are understood solely through its NLD and by how conceptual functions interact with it.

**Example**:

```
CONCEPT
	TYPE APIAuthToken := {{An opaque security token string required for authenticating calls to ExternalServiceX.}}
	
	TYPE RawSensorInput :=
		{{A block of binary data received directly from a hardware sensor before any parsing or interpretation.
		  The format is specific to the sensor hardware, documented externally.}}
	ENDTYPE
ENDCONCEPT
```

## Conceptual Records (`RECORD`)

Use `RECORD` when the conceptual type is best understood as a data aggregate or structure with named fields.

**Syntax (Single-line):**

```
RECORD RecordName := {{NLD describing conceptual fields and structure}} [ENDRECORD]
```

The `ENDRECORD` keyword is optional if the entire declaration is on a single line.

**Syntax (Multi-line NLD):**

```
RECORD RecordName :=
	{{NLD describing this as a data aggregate. The NLD **must** outline the
	  minimal, necessary conceptual fields and their AlgoDraft-compatible types.
	  Example: Expected fields include '$id' (Integer), '$status' (String).
	  The realized structure in a target language or external system may be a superset.}}
ENDRECORD
```

Signals that `RecordName` is conceptually a structured data type. The NLD specifies the essential conceptual fields.

**Example:**

```
CONCEPT
	RECORD Coordinates := {{A 2D point. Minimal fields: 'xPos' (Float), 'yPos' (Float).}}

	RECORD MessageHeader :=
		{{Header information for a network message.
		  Minimal fields: 'messageID' (String), 'senderID' (String),
		  'timestamp' (DateTime), 'priority' (Integer).}}
	ENDRECORD
ENDCONCEPT

$msg AS MessageHeader <- NEW MessageHeader(
	"msg103",
	"boss",
	NEW DateTime(2025, 5, 18, 12, 0, 0),
	1)
```

## Conceptual Classes (`CLASS`)  

Use `CLASS` when the conceptual type is best understood as an entity with state and behavior (methods), aligning with object-oriented concepts.

**Syntax (Single-line):**

```
CLASS ClassName := {{NLD describing conceptual state, methods, and OO nature}} [ENDCLASS]
```

The `ENDCLASS` keyword is optional if the entire declaration is on a single line.

**Syntax (Multi-line NLD):**

```
CLASS ClassName :=
	{{NLD describing this as an entity with internal state and behaviors (methods).
	  The NLD MUST outline the minimal, necessary conceptual methods, including their
	  intended signatures (parameter types, return type).
	  Example: Conceptual methods: Initialize(), ProcessItem(IN item AS ItemType).
	  The realized class may have additional methods or state.}}
ENDCLASS
```

Signals that `ClassName` is conceptually an object-oriented entity. The NLD specifies essential conceptual methods.

**Example:**

```
CONCEPT
    CLASS UserAuthenticator := {{Manages user authentication against an external system. Conceptual method: Authenticate(IN user, IN pass) -> AuthToken.}}

    CLASS ReportBuilder :=
        {{An object responsible for constructing a complex report.
          Conceptual methods: AddSection(IN title AS String), AddParagraph(IN text AS String),
          GenerateReport() -> ReportDocumentType.
          Manages internal state representing the report being built.}}
    ENDCLASS
ENDCONCEPT
```

# Declaring Conceptual External Constants

Used for named constant values whose actual definition or origin is external.

**Syntax (Single-line):**

```
CONST $CONST_NAME AS DataType := {{NLD describing the constant}} [ENDCONST]
```

The `ENDCONST` keyword is optional for single-line declarations.

**Syntax (Multi-line NLD):**

```
CONST $CONST_NAME AS DataType :=
	{{A detailed NLD explaining the constant's significance, how its value
	  is determined or where it originates externally, and any important context.}}
ENDCONST
```

Declares `$CONST_NAME` as a constant within AlgoDraft, having a specific AlgoDraft `DataType`. Its fixed value is understood to be defined by the external source described in the NLD. The `AS DataType` clause is crucial for AlgoDraft to know how this constant can be used in expressions.

**Example**:

```
CONCEPT
    CONST $MAX_FILE_UPLOAD_SIZE_MB AS Integer := {{The maximum file upload size in megabytes, as dictated by server configuration.}}
    
    CONST $APPLICATION_VERSION_STRING AS String :=
        {{The current version string of the external application framework,
          e.g., "v2.3.1-beta". Used for logging and compatibility checks.}}
    ENDCONST
ENDCONCEPT
```

# Declaring Conceptual External Functions  

Used for functions whose implementation logic is considered external, abstract, or will be defined separately.

**Syntax (Single-line):**

```
FUNCTION <func_signature> := {{NLD describing the function's behavior and external nature}} [ENDFUNCTION]
```

The `ENDFUNCTION` keyword is optional for single-line declarations.

**Syntax (Multi-line NLD):**

```
FUNCTION <func_signature> := // Signature ends with :=
	{{A detailed NLD describing the function's overall behavior, its interaction
	  with external systems or data, any significant side effects,
	  error conditions, and the meaning of its parameters and return value.}}
ENDFUNCTION
```

A full AlgoDraft-compatible <func_signature> (name, parameters including optional directionality `IN`/`OUT`/`INOUT` and types, and return type) is **mandatory**. The `:= {{NLD}}` then provides the complete conceptual definition of this function's behavior and contract as an abstract or external operation.

**Example**:

```
CONCEPT
    FUNCTION GetSystemTimeUTC() -> DateTime := {{Retrieves the current Coordinated Universal Time from the underlying operating system or a time server.}}

    FUNCTION ValidateUserCredentials (
			IN $username AS String,
			IN $encryptedPassword AS String, // Assuming password is pre-encrypted
			OUT $userProfile AS UserProfile, // UserProfile is a CONCEPTUAL TYPE
			OUT $lastLogin AS DateTime
		) -> Boolean := // Returns TRUE on successful validation
        {{Checks the provided username and encrypted password against an external
          identity management system. If valid, populates $userProfile and $lastLogin
          from the system and returns TRUE. Otherwise, returns FALSE.}}
    ENDFUNCTION
ENDCONCEPT
```

# Using Conceptual Entities in AlgoDraft Logic

Entities declared within `CONCEPT` blocks are used in your standard AlgoDraft functions just like any other defined type, constant, or function. The NLDs associated with these conceptual entities guide the designer on the assumptions they can make when using them.

**Example 1:**

```
// Assumes declarations from previous CONCEPT examples are in scope

FUNCTION PerformUserLogin($user AS String, $pass AS String) -> Boolean :=
    $profile AS UserProfile        // Using a CONCEPTUAL TYPE
    $lastLoginTime AS DateTime     // Using a built-in or CONCEPTUAL TYPE
    $isValid AS Boolean
    $encryptedPass AS String

    $encryptedPass <- EncryptPasswordLocally($pass) // Assume EncryptPasswordLocally is an AlgoDraft function

    // Using a CONCEPTUAL FUNCTION
    $isValid <- ValidateUserCredentials($user, $encryptedPass, $profile, $lastLoginTime)

    IF $isValid THEN
        NOTIFY {{show a welcome message for $profile.$displayName}} // Interaction with conceptual record field
        NOTIFY {{last login time as $lastLoginTime}}
        IF GetSystemTimeUTC().$year > 2050 THEN // Using a CONCEPTUAL FUNCTION
             PRINT {{the future is now!}}
        ENDIF
        RETURN TRUE
    ELSE
        NOTIFY {{login failed for user $user}}
        IF {{$user is admin}} THEN
            LogAdminLoginFailure($SERVICE_API_URL) // $SERVICE_API_URL is a CONCEPTUAL CONST
        ENDIF
        RETURN FALSE
    ENDIF
ENDFUNCTION
```

In this example:

- `UserProfile` (even if conceptual) is used for declarations.

- `ValidateUserCredentials`, `GetSystemTimeUTC` are called as conceptual functions.

- `$SERVICE_API_URL` is used as a conceptual constant.

**Example 2: Library-Agnostic Graphics:**

```AlgoDraft
// The following code 
CONCEPT
	TYPE MdRoot := {{the root element in the markdown AST hierarchy}}
	
	TYPE MdElement := {{the base element of all elements in the Markdown AST hierarchy}}
ENDCONCEPT

$pthMdFile <- INPUT {{the path/url of a markdown document}}
TRY
	$mdFile AS File<String> <- DO {{open the Markdown document in text, read mode}}
	$ast as MdRoot <- DO {{parse $mdFile into Markdown AST}}
CATCH {{reading document failure}} AS $err:
	NOTIFY {{the $err error occurred while reading $pthMdFile}}
	DO {{exit}}
CATCH {{Markdown AST parsing failure}}:
	NOTIFY {{cannot parse $pthMdFile}}
	DO {{exit}}
ENDTRY
FOR EACH ($elem AS MdElement) IN $ast.$children DO
    // Do something with $elem...
ENDFOR
```
