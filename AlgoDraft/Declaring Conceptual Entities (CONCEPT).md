---
status: Draft
---
# What are `CONCEPT` Blocks?

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

- **Syntax (Multi-line NLD):**
    
    ```
    RECORD RecordName :=
        {{NLD describing this as a data aggregate. The NLD **must** outline the
          minimal, necessary conceptual fields and their AlgoDraft-compatible types.
          Example: Expected fields include 'id' (Integer), 'status' (String).
          The realized structure in a target language or external system may be a superset.}}
    ENDRECORD
    ```
    
    content_copydownload
    
    Use code [with caution](https://support.google.com/legal/answer/13505487).Pseudocode
    
- **Explanation:** Signals that RecordName is conceptually a structured data type. The NLD specifies the essential conceptual fields. AlgoDraft itself doesn't provide direct field access syntax (e.g., myRecord.field) for these conceptual records; interaction with fields would typically be through conceptual functions or DO {{NLD}} statements that describe such access.
    
- **Example:**
    
    ```
    CONCEPT
        RECORD Coordinates := {{A 2D point. Minimal fields: 'xPos' (Float), 'yPos' (Float).}}
    
        RECORD MessageHeader :=
            {{Header information for a network message.
              Minimal fields: 'messageID' (String), 'senderID' (String),
              'timestamp' (DateTime), 'priority' (Integer).}}
        ENDRECORD
    ENDCONCEPT
    ```
    
    content_copydownload
# Declaring Conceptual External Constants

These named constants have an AlgoDraft type, but their value is understood to originate from an external source described by the NLD.

**Syntax (Single-line):**

```
CONST $CONST_NAME AS DataType := {{NLD describing the constant's meaning and origin}} [ENDCONST]
```

The `ENDCONST` keyword is optional for single-line declarations.

**Syntax (Multi-line NLD):**

```
CONST $CONST_NAME AS DataType :=
    {{A detailed NLD for the constant, explaining its significance,
      how its value is determined externally, and any constraints.}}
ENDCONST
```

**Example**:

```
CONCEPT
    CONST $MAX_LOGIN_ATTEMPTS AS Integer := {{The maximum number of login attempts allowed by the external authentication policy.}}

    CONST $SERVICE_API_URL AS String :=
        {{The base URL for the primary external service this algorithm interacts with.
          The specific endpoint is defined by the "ExternalServiceAPI" documentation.}}
    ENDCONST
ENDCONCEPT
```

# Declaring Conceptual External Functions  

For conceptual external functions, you **must** provide a full AlgoDraft-compatible signature. The NLD then describes what this function, with this specific interface, achieves externally.

**Syntax (Single-line):**

```
FUNCTION <func_signature> := {{NLD describing the function's behavior and external nature}} [ENDFUNCTION]
```

The `ENDFUNCTION` keyword is optional for single-line declarations.

**Syntax (Multi-line NLD):**

```
FUNCTION <func_signature> := // Signature ends with :=
    {{A detailed NLD for the function, outlining its purpose,
      interaction with external systems, and any important side effects
      or error conditions not captured by the AlgoDraft signature.}}
ENDFUNCTION
```

**Example**:

```
CONCEPT
    FUNCTION LogSystemEvent(IN $message AS String, IN $severityLevel AS Integer) := {{Records an event message to an external system-wide log.}}

    FUNCTION AuthenticateUser (
	        IN $username AS String,
	        IN $password AS String,
	        OUT $sessionData AS UserSession // UserSession is a conceptual type
	    ) -> Boolean := // Returns TRUE on successful authentication
        {{Validates user credentials against an external identity provider.
          If successful, populates $sessionData and returns TRUE; otherwise returns FALSE.}}
    ENDFUNCTION
ENDCONCEPT
```

# Examples

**Example 1: Library-Agnostic Graphics:**

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
	NOTIFY {{the $err error occurred reading $pthMdFile}}
	DO {{exit}}
CATCH {{Markdown AST parsing failure}}:
	NOTIFY {{cannot parse $pthMdFile}}
	DO {{exit}}
ENDTRY
FOR EACH ($elem AS MdElement) IN $ast.$children DO
    // Do something with $elem...
ENDFOR
```
