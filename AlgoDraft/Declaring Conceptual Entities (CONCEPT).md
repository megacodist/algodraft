---
status: Draft
---
In AlgoDraft, algorithms often need to interact with or refer to ideas, data structures, functionalities, or constant values whose precise, low-level implementation details are either unknown at the early design stage, intentionally kept abstract, external to the core AlgoDraft logic, or perhaps even beyond AlgoDraft's direct expressive capabilities for that specific detail.

The **`CONCEPT ... ENDCONCEPT`** block is AlgoDraft's dedicated mechanism for declaring these **conceptual entities**. It allows you to give these abstract notions a name and a clear, human-understandable specification within your algorithm design.

**The Role of Natural Language Descriptions (NLDs) within `CONCEPT`:**

Every entity declared within a `CONCEPT` block is defined using the `:= {{description}}` syntax. The Natural Language Description (NLD), enclosed in `{{ }}`, provides the complete conceptual definition for AlgoDraft's purposes. It describes the entity's essential nature, its expected behavior or contract, its origin (if conceptualized as external), or the minimal interface it's expected to present (especially for conceptual records and classes).

**Message to Implementers/Designers & Realization Paths:**

Entities declared in `CONCEPT` blocks are abstract placeholders that signal a requirement. They await "realization" as the design progresses towards a concrete implementation. This signals to anyone reading or implementing the AlgoDraft design:

"Dear implementer/future designer, the following entity represents a need within this algorithm. AlgoDraft, by design, might not express the full depth of this external system or complex type, or I (the designer) am choosing to keep it abstract for now. When moving towards concrete implementation, this concept needs to be addressed. This might involve one of three primary realization paths:

1. **Implementation in AlgoDraft:** The entity (e.g., a CONCEPT FUNCTION) is fully implemented using standard AlgoDraft statements and structures. The CONCEPT declaration serves as its initial specification and contract.
    
2. **Integration with External Libraries/APIs/Systems:** The entity corresponds to functionality or data provided by an external software library, operating system feature, hardware interface, or remote service. This often involves finding and using appropriate APIs from that external source. (This path might later be documented in AlgoDraft using IMPORT blocks if the design is refined to that level of detail).
    
3. **Leveraging Target Programming Language Capabilities:** The chosen target programming language (for final implementation beyond AlgoDraft) may offer built-in features, types, or paradigms that directly or effectively realize the conceptual entity.
    

Your task is to choose or combine these paths appropriately to fulfill the conceptual requirement based on project needs and available technologies."





The `CONCEPT` block is a design tool for declaring conceptual entities, although the implementers are highly likely to find "similar stuff out there." It allows you to define AlgoDraft names for entities whose precise nature and behavior are specified using Natural Language Descriptions (NLDs).

**Purpose and Philosophy:**

- **Focus:** Design-time abstraction, OS/library agnosticism, clarity of high-level dependencies.

- **Mechanism:** You declare an AlgoDraft name (for a type, constant, or function) and associate it with an NLD using the definition operator (`:=`). The NLD is the conceptual definition for AlgoDraft's purposes.

**Syntax**:

```
CONCEPT
    // Declarations of conceptual types, constants, and functions using ':= {{NLD}}'
ENDCONCEPT
```

Within this block, each declaration clearly states its AlgoDraft name and type/signature, followed by `:=` and its descriptive NLD.

# Declaring Conceptual External Types

Conceptual types are treated as opaque by AlgoDraft; their properties and usage are understood through their NLD and how conceptual external functions operate on them.

**Syntax (Single-line):**

```
TYPE TypeName := {{NLD describing the type's concept}} [ENDTYPE]
```

The `ENDTYPE` keyword is optional if the entire declaration is on a single line.

**Syntax (Multi-line NLD):**

```
TYPE TypeName :=
    {{A more detailed NLD, potentially spanning multiple lines,
    describing the characteristics and purpose of this conceptual type.}}
ENDTYPE
```

**Example**:

```
CONCEPT
    TYPE FileHandle := {{An opaque system identifier for an open file, used for I/O operations.}}

    TYPE UserSession :=
        {{A conceptual data structure representing an active user session,
          containing details like user ID, login time, and session token.
          Specific fields are externally defined.}}
    ENDTYPE
ENDCONCEPT
```

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
