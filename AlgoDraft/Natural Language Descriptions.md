---
status: Draft
---
NLD is a syntax in AlgoDraft but not like others. It is a pillar that the power and flexibility of AlgoDraft is based upon. It makes it nicely possible to embed human ideas directly into strict programming constructs.



A core feature of AlgoDraft is the use of human language phrases and sentences known as **Natural Language Descriptions (NLDs)**. These are short, descriptive pieces of text, written in plain English (or another natural language), enclosed in double curly braces `{{ }}` that follow specific AlgoDraft keywords.

NLDs are the points where AlgoDraft explicitly connects the human way of thinking about processes with the structured requirements of algorithm design. They act as placeholders for more detailed logic or descriptions of intent.

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

# Why Use NLDs?

Humans naturally think and communicate using human languages, often describing processes at a high level. Computers, however, operate based on precise, formal programming languages. AlgoDraft aims to *reconcile these two worlds*, acting as a bridge between human thought and programmable logic.

NLDs, enclosed in `{{ }}`, are fundamental to this mission. They are the designated places within AlgoDraft where you express parts of the algorithm using natural language placeholders. In other words, `{{ }}` consistently signals "this is a descriptive, human-readable placeholder/description that needs refinement or interpretation."

This approach offers several key advantages:
1. **Focus on Logic First:** NLDs allow you to focus on what needs to happen (e.g., `DO {{sort the list}}`) before getting bogged down in exactly how it will be coded in a specific programming language.

2. **Abstraction:** They provide a way to abstract complex operations (like file I/O, user input, complex calculations, error conditions, or even conditional checks in early drafts) into simple, understandable descriptions.

3. **Clarity and Readability:** Algorithms using clear NLDs can be easier to read and understand, especially during the initial design phases or for non-programmer stakeholders.

4. **Ease of Drafting:** It's often faster and more intuitive to draft the initial steps of an algorithm using `{{natural language descriptions}} `rather than immediately writing formal code or precise logic.

5. **Structured Bridge to Code:** While flexible, NLDs occur within the structured context of AlgoDraft keywords (`DO`, `INPUT`, `NOTIFY`, `RAISE`, `IF`, etc.), making the transition from pseudocode to actual code smoother once the NLDs are sufficiently refined.

In essence, NLDs make AlgoDraft a powerful tool for drafting algorithms in a way that feels natural while still providing the structure needed for eventual implementation.

# Using NLDs in AlgoDraft

While NLDs use natural language, they function as specific placeholders within the AlgoDraft syntax. NLDs are enclosed in double curly braces: `{{description}}`. They typically follow an AlgoDraft keyword.

```
KEYWORD {{natural language description}}
$variable <- KEYWORD {{natural language description yielding a value}}
```

# Key Characteristics of NLDs

While NLDs use natural language, they are not completely free-form. They operate within specific guidelines defined by the keyword they follow:

* **No Upper Case**: NLDs must be written in lower case as much as possible. Avoid sentence casing or title casing.

* **Integration of Variables**: NLDs can and often should include AlgoDraft variable names (e.g., `$userName`, `$userData`) to clearly link the described action, data, or event to the algorithm's state.

* **Clarity**: An NLD should be unambiguous and clearly convey its intended meaning to the audience.

* **Conciseness**: While descriptive, it should be reasonably brief and to the point.

* **Non-Executable Description**: Remember, NLDs are descriptions for humans (and for guiding later coding). They are not directly executable code themselves.

# NLDs Best Practices
Use them when:
- You want to **express intent clearly** without diving into implementation details.

- The meaning is **obvious or already explained elsewhere**.

- You're aiming for **readability or teaching purposes**.

Avoid them:
- If you're refining the algorithm for implementation.

- When the meaning could be **ambiguous** or **context-sensitive**.

- If your pseudocode is meant to be machine-parsable (e.g., for auto-grading tools or interpreters).