---
status: Draft
---
A defining characteristic and a cornerstone of AlgoDraft's design philosophy is the **Natural Language Description (NLD)**. While NLDs are a specific syntactic element – short, descriptive pieces of text in human languages enclosed in double curly braces (`{{ }}`) – they are unlike any other construct in the language. NLDs stand as a fundamental pillar upon which the power and flexibility of AlgoDraft are built.

They uniquely enable the direct embedding of human thought and high-level conceptualization within the structured framework of an algorithmic design. NLDs serve as the crucial **junction** where the often informal, flexible, and sometimes imprecise nature of human thought meets the formal, deterministic requirements of programming languages and algorithm specification. They are the designated placeholders where AlgoDraft explicitly connects the human way of thinking about processes with the structured elements needed for clear design.

# Types of NLDs

Currently NLDs fulfill two primary roles within the AlgoDraft syntax:

## Inline Descriptive NLDs

Used within AlgoDraft statements such as `INPUT`, `DO`, `IF`, `NOTIFY`, or `RAISE`. In this role, NLDs specify an atomic piece of data being sought, an action to be performed, a condition to be evaluated, or an event/exception being signaled, all in human-readable terms. They allow designers to sketch out procedural steps without immediately detailing the precise AlgoDraft logic.

**Example**
```
// Example NLDs using the {{nld}} syntax:

// NLD describing data needed...
$userName AS String <- INPUT {{the user's full name}}

// NLD describing an action...
$fileHandle AS File <- DO {{open the config file in binary for reading}}

// NLD describing an event...
NOTIFY {{file loaded successfully as success}}

// NLD describing an exception...
RAISE {{required configuration value missing}}

// NLDs describing an action and a placeholder type...
$ast AS {{AST node}} <- DO {{parse the markdown text}}

// NLDS describing a condition...
IF {{there is hardware error}} THEN
   DO {{exit program with code 2}}
ELSE
```

## Conceptual Definitional NLDs

Used exclusively within `CONCEPT` blocks, paired with the definition operator (`:=`). Here, NLDs provide the complete conceptual definition for abstract or external types, constants, and functions. They describe the essential nature, contract, or behavior of these entities whose concrete implementation lies outside the immediate AlgoDraft definition or is being deferred.

```
CONCEPT
	RECORD UserInfo :=
		{{holds information about user including name, id, passHash in the OS}}
	ENDRECORD
	
	CLASS MdRoot :=
		{{the root element which holds the entire markdown hierarchy}}
	ENDCLASS

    CONST $DEFAULT_TIMEOUT AS Integer :=
        {{the recommended timeout in milliseconds for calls to the WeatherService API, as per its documentation}}
    ENDCONST

    FUNCTION GetWeatherData (IN $location AS String) -> ExternalAPIResponse :=
        {{retrieves current weather data for the specified location by calling the external WeatherService API}}
    ENDFUNCTION
ENDCONCEPT
```
# Why Use NLDs?

Humans naturally think and communicate using human languages, often describing processes at a high level. Computers, however, operate based on precise, formal programming languages. AlgoDraft aims to reconcile these two worlds, acting as a bridge between human thought and programmable logic.

NLDs, enclosed in {{ }}, are fundamental to this mission. They are the designated places within AlgoDraft where you express parts of the algorithm or define conceptual entities using natural language. In other words, {{ }} consistently signals "this is a descriptive, human-readable placeholder, specification, or description that needs further refinement, implementation, external realization, or interpretation."

This approach offers several key advantages:

1. **Focus on Logic First (for Inline NLDs):** Inline NLDs allow you to focus on what needs to happen (e.g., `DO {{sort the list}}`) before getting bogged down in exactly how it will be coded in AlgoDraft statements.

2. **Define Conceptual Entities (for Definitional NLDs):** NLDs within `CONCEPT` blocks allow you to define the nature and contract of types, constants, and functions that are external or abstract, without specifying their internal AlgoDraft implementation.

3. **Abstraction:** They provide a way to abstract complex operations (like file I/O, user input, intricate calculations, error conditions, or even conditional checks in early drafts) and external systems into simple, understandable descriptions.

4. **Clarity and Readability:** Algorithms using clear NLDs can be easier to read and understand, especially during the initial design phases or for non-programmer stakeholders.

5. **Ease of Drafting:** It's often faster and more intuitive to draft initial algorithm steps or define external dependencies using `{{natural language descriptions}}` rather than immediately writing formal code or precise logic.

6. **Structured Bridge to Code/Implementation:**
    
    - **For inline NLDs**: While flexible, they occur within the structured context of AlgoDraft keywords (`DO`, `INPUT`, etc.), making the transition to AlgoDraft statements smoother once the NLDs are refined.
    
    - **For definitional NLDs in `CONCEPT` blocks**: They provide a clear specification for entities that will later be implemented in AlgoDraft or mapped to concrete external libraries (e.g., via `IMPORT`).

In essence, NLDs make AlgoDraft a powerful tool for drafting algorithms and defining conceptual interfaces in a way that feels natural while still providing the structure needed for eventual detailed design and implementation.

# Using NLDs in AlgoDraft

NLDs are enclosed in double curly braces: `{{description}}`. Their specific usage depends on the context:

1. **Inline NLDs:** Typically are used as a placeholder within an AlgoDraft statement describing an action, input, event, condition, or exception.
    
    ```
    KEYWORD {{natural language description of action/condition/etc.}}
    $variable <- KEYWORD {{natural language description yielding a value}}
    ```

2. **Definitional NLDs:** Used with the definition operator (`:=`) within a `CONCEPT` block to define the nature of a conceptual type, constant, or function.
    
    ```
    CONCEPT
        TYPE TypeName := {{NLD defining the conceptual type}}
        CONST $ConstName AS DataType := {{NLD defining the conceptual constant}}
        FUNCTION FunctionSignature := {{NLD defining the conceptual function's behavior}}
    ENDCONCEPT
    ```

# Key Characteristics of NLDs

Regardless of their role, NLDs should adhere to certain guidelines:

- **Lowercase Convention**: NLDs should primarily be written in lowercase. Avoid sentence casing or title casing, unless referring to proper nouns or established names.

- **Integration of Variables (for Inline NLDs primarily):** Inline NLDs can and often should include AlgoDraft variable names (e.g., `$userName`, `$userData`) to clearly link the described action, data, or event to the algorithm's state.

- **Clarity**: An NLD should be unambiguous and clearly convey its intended meaning to the audience.

- **Conciseness**: While descriptive, it should be reasonably brief and to the point.

- **Non-Executable Description**: Remember, NLDs are descriptions for humans (and for guiding later coding or external realization). They are not directly executable AlgoDraft code themselves.

# NLDs Best Practices

**Use inline NLDs when:**

- You want to **express the intent of a specific step clearly** without immediately diving into its detailed AlgoDraft implementation.

- The specific action or data is **obvious in context or will be detailed later**.

- You're aiming for **initial draft readability or teaching purposes**.

**Use definitional NLDs when:**

- You are **defining a type, constant, or function that is conceptual, abstract, or external** to your current AlgoDraft logic.

- You want to **specify the contract or behavior of these conceptual entities** without providing an AlgoDraft implementation for them.

# When to Replace or Realize NLDs

- **Inline NLDs:**

	- As the algorithm design is refined, inline NLDs (like `IF {{a condition is fullfiled}} THEN`) should generally be replaced with actual AlgoDraft statements that perform the described action.
	
	- **Transpilation**: When pseudocode is being transpiled to the destination language like  `DO {{sort the list}}` is replaced with the target language sorting APIs.

- **Definitional NLDs in CONCEPT blocks:** These represent entities that need "realization." This means either:
    
    - Providing a standard AlgoDraft definition for them (e.g., writing an AlgoDraft `FUNCTION` body for a `CONCEPT FUNCTION`).
    
    - Mapping them to concrete external entities, potentially through an `IMPORT` block.

Avoid leaving NLDs in a state where their meaning could be **ambiguous** or **critically context-sensitive** without sufficient surrounding explanation, especially in final design documents. If your pseudocode is meant for any form of automated processing, the interpretation of NLDs would be a significant challenge unless very constrained.