# Introduction
Humans need to convey their ideas or exchange ideas with other people. If you want to talk about mathematics, you need to know algebra, set theory, their theorems, jargons among other things. Pseudocode is the language of algorithms.

It is the general way to express algorithmic ideas.

It is worth mentioning that there is NOT a standard pseudocode that everyone agrees upon.

AlgoDraft is the result of our effort to have the most formal-informal algorithm language: the most formal among all casual contracts and the most informal among all programming languages.

The primary goal of pseudocode is clear, unambiguous, and easy-to-write/read communication of logic.

#### Characteristics
Pseudocode ia all about logic not implementation.

It must be as higher level as possible if we do not want to say the highest level. The ligh level in programming sense means free from any limiting technology, assuming everything is possible.

pseudocode prioritizes clarity and simplicity over strict syntax.

Pseudocode's primary goal is clear communication of logic.

Pseudocode must aim at a broad audience or focusing on procedural steps, I would lean towards `$foo AS Integer` because its explicitness prioritizes immediate understanding over symbolic elegance.

With collaboration with Google Gemini, I (Megacodist) managed to outlined this pseudocode named AlgoDraft with file extension of .`algod`.

#### Benefits
1.	Pseudocode is a way to think out of the box.
2.	It enables everyone to not limit your train of thoughts to any programming language or framework.
3.	It is possible to employ different levels of abstraction. For example, knowing a prefix in string manipulation is a substring starting from the beginning, we can have all of the following:
Leading 🡨 Obtain

# Data types
## Boolean Type
Variables of this type can hold only `TRUE` and `FALSE` values.

## Integer Type

## String Type
Strings are 0-based-indexed sequences of characters.

## Real Type

## FsPath Type
Represents a class to instantiate objects to work with file system items.

## File Type
An instance of this type represents an opened file ready for I/O operations.

### Methods
#### Opening

```
$configFile <- DO: open the file $filePath in text, read mode
```

#### Closing
```
DO: close $configFile
```

## Function Type
Any defined procedure and function name is of `Function` type. This is particularly useful for callbacks.

## Stream Type

# Basic syntax

## Variables, Constants, and Assignment
To work with data in AlgoDraft, you need places to store it. This is done using **variables** and **constants**. **Assignment** is the process of placing a value into a variable or constant.

### Variables
Variables are named containers used to store data that might change during the execution of your algorithm. Think of them as labeled boxes where you can put information and update it later. Before you can use a variable, you must declare it.

#### Variable Declaration Syntax
Declaration specifies the variable's name and the type of data it will hold:
```
$variableName AS DataType
```

* **`$`**: All variable names must start with a dollar sign ($).

* **`variableName`**: The name you choose for the variable. By convention, use [camelCase](#camelcase) (e.g., `userAge`, `firstName`, `totalScore`).

* **`AS DataType`**: Specifies the type of data the variable can store (e.g., `Integer`, `String`, `Boolean`, `List<Real>`). This is mandatory.

#### Example

```
$counter AS Integer
$customerName AS String
$isActive AS Boolean
```

### Assignment

The assignment operator `<-` is used to place a value into a declared variable.

#### Syntax
```
$variableName <- <value>
```

* **`<-`**: The assignment operator. Read it as "gets" or "is assigned". It takes the value on the right and stores it in the variable on the left.

* **`<value>`**: The data being assigned. This can be a literal value (like `10`, `"Hello"`, `TRUE`), another variable, or the result of an operation. The value must be compatible with the variable's declared DataType.

#### Example
```
$counter AS Integer // Declaration
$counter <- 0       // Assignment

$customerName AS String
$customerName <- "Bob"

$isActive AS Boolean
$isActive <- TRUE
```

#### Declaration with Initialization Syntax
You can declare a variable and assign its initial value in a single step.
```
$variableName AS DataType <- <initialValue>
```

#### Example
```
$score AS Integer <- 0
$message AS String <- "Welcome!"
$isComplete AS Boolean <- FALSE
```

#### Changing Variable Values

The key characteristic of variables is that their value can be updated using the assignment operator again.

```
$score AS Integer <- 0
NOTIFY: Initial score is $score as info // Notifies: Initial score is 0...

$score <- $score + 10 // Update the value
NOTIFY: Score after update is $score as info // Notifies: Score after update is 10...
```

### Constants
Constants are named containers used to store data that will not change after it's initially defined. They are useful for fixed values like mathematical constants (Pi), configuration settings (maximum attempts), or status codes. Constants must be defined using the `CONST` keyword and must be assigned a value immediately.

#### Syntax:
```
CONST $CONSTANT_NAME AS DataType <- <value>
```
* **`CONST`**: The keyword indicating that this is a constant.

* **`$`**: Constant names also start with a dollar sign ($).

* **`CONSTANT_NAME`**: The name you choose for the constant. By convention, use [UPPER_SNAKE_CASE](#screaming_snake_case-upper_snake_case-or-macro_case) (e.g., `MAX_RETRIES`, `PI`, `DEFAULT_TIMEOUT`).

* **`AS DataType`**: Specifies the fixed data type.

* **`<- <value>`**: The assignment operator is used here only to set the constant's initial (and final) value. This value must be provided at definition time.

#### Example
```
CONST $PI AS Real <- 3.14159
CONST $MAX_LOGIN_ATTEMPTS AS Integer <- 3
CONST $ADMIN_EMAIL AS String <- "admin@example.com"
```

#### Immutability

Once a constant is defined, you cannot change its value. Attempting to assign a new value to a constant is an error in concept.

```
CONST $MAX_USERS AS Integer <- 100

// The following would be conceptually WRONG in AlgoDraft:
// $MAX_USERS <- 101 // ERROR: Cannot assign to a constant
```

### The Assignment Operator (`<-`)
The `<-` operator is central to both variables and constants:

* **For Variables**: Used both to set the initial value and to update the value later.

* **For Constants**: Used only to set the initial (and permanent) value during definition with the CONST keyword.

It always means "take the value from the right side and store it into the named container on the left side."

| Feature | Variable ($varName) | Constant (CONST $CONST_NAME) |
| ------- | ------------------- | ----------------------------- |
|Purpose | Store changing data | Store fixed data |
| Keyword | None (just `$`) | `CONST` |
| Naming | `$camelCase` | `$UPPER_SNAKE_CASE` |
| Type |	`AS DataType` (Required) | `AS DataType` (Required) |
| Assignment | `<-` (For initialization & updates) | `<-` (Required only at definition) |
| Mutability | Mutable (Can change) | Immutable (Cannot change after definition) |

## Comments
We are using `//` or `✍` to start comments.

## Keywords
This version of pseudocode has lots of keywords and they must be typed in all uppercase. All uppercase is reserved for keywords.

## Assignments
Assignment is used to assign a new value to a variable. Any left-pointing character or a combination of two that has the bolder appearance is acceptable:
```
$var AS Integer
$var <- 2
$var ◀━ 3
```

## Natural Language Descriptions (NLD)
A core feature of AlgoDraft is the use of human language phrases and sentences known as **Natural Language Descriptions (NLDs)**. These are short, descriptive pieces of text, written in plain English (or another natural language), enclosed in double curly braces `{{ }}` that follow specific AlgoDraft keywords.

NLDs are the points where AlgoDraft explicitly connects the human way of thinking about processes with the structured requirements of algorithm design. They act as placeholders for more detailed logic or descriptions of intent.

#### Example
```
// Example NLDs using the {{NLD}} syntax:

// NLD describing data needed
$userName AS String <- INPUT {{the user's full name}}

// NLD describing an action
$fileHandle AS File <- DO {{open the config file in binary for reading}}

// NLD describing an event
NOTIFY {{file loaded successfully as success}}

// NLD describing an exception
RAISE {{required configuration value missing}}

// NLDs describing an action and a placeholder type
$ast AS {{AST node}} <- DO {{parse the markdown text}}
```

#### Why Use NLDs?

Humans naturally think and communicate using human languages, often describing processes at a high level. Computers, however, operate based on precise, formal programming languages. AlgoDraft aims to *reconcile these two worlds*, acting as a bridge between human thought and programmable logic.

NLDs, enclosed in `{{ }}`, are fundamental to this mission. They are the designated places within AlgoDraft where you express parts of the algorithm using natural language placeholders. In other words, `{{ }}` consistently signals "this is a descriptive, human-readable placeholder/description that needs refinement or interpretation."

This approach offers several key advantages:
1. **Focus on Logic First:** NLDs allow you to focus on what needs to happen (e.g., `DO {{sort the list}}`) before getting bogged down in exactly how it will be coded in a specific programming language.

2. **Abstraction:** They provide a way to abstract complex operations (like file I/O, user input, complex calculations, error conditions, or even conditional checks in early drafts) into simple, understandable descriptions.

3. **Clarity and Readability:** Algorithms using clear NLDs can be easier to read and understand, especially during the initial design phases or for non-programmer stakeholders.

4. **Ease of Drafting:** It's often faster and more intuitive to draft the initial steps of an algorithm using `{{natural language descriptions}} `rather than immediately writing formal code or precise logic.

5. **Structured Bridge to Code:** While flexible, NLDs occur within the structured context of AlgoDraft keywords (`DO`, `INPUT`, `NOTIFY`, `RAISE`, `IF`, etc.), making the transition from pseudocode to actual code smoother once the NLDs are sufficiently refined.

In essence, NLDs make AlgoDraft a powerful tool for drafting algorithms in a way that feels natural while still providing the structure needed for eventual implementation.

#### Using NLDs in AlgoDraft

While NLDs use natural language, they function as specific placeholders within the AlgoDraft syntax. NLDs are enclosed in double curly braces: `{{description}}`. They typically follow an AlgoDraft keyword.

```
KEYWORD {{natural language description}}
$variable <- KEYWORD {{natural language description yielding a value}}
```

#### Key Characteristics of NLDs

While NLDs use natural language, they are not completely free-form. They operate within specific guidelines defined by the keyword they follow:

* **No Upper Case**: NLDs must be written in lower case as much as possible. Avoid sentence casing or title casing.

* **Integration of Variables**: NLDs can and often should include AlgoDraft variable names (e.g., `$userName`, `$userData`) to clearly link the described action, data, or event to the algorithm's state.

* **Clarity**: An NLD should be unambiguous and clearly convey its intended meaning to the audience.

* **Conciseness**: While descriptive, it should be reasonably brief and to the point.

* **Non-Executable Description**: Remember, NLDs are descriptions for humans (and for guiding later coding). They are not directly executable code themselves.

#### ✍️ When to use natural language conditions

Use them when:

- You want to **express intent clearly** without diving into implementation details.

- The meaning is **obvious or already explained elsewhere**.

- You're aiming for **readability or teaching purposes**.

#### ❗When to be precise instead:

- If you're refining the algorithm for implementation.

- When the meaning could be **ambiguous** or **context-sensitive**.

- If your pseudocode is meant to be machine-parsable (e.g., for auto-grading tools or interpreters).

## Performing Natural Language Actions (`DO`)

Sometimes we need to describe an action rather than imperatively coding it. In such cases we use **Natural Language Statements** (NLSes). The action:

* usually involves complexity better described in natural language than with simple operators.

* must be expressed via an NLD employing imperative language.

* either results in a value that needs to be assigned to a variable or causes side effect.

#### Syntax (Action-Only):
```
DO {{imperative_nld_sentence}}
```

```
DO {{sort $mylist}}
DO {{add 3 to $idx}}
```

#### Syntax (Value-Yielding)
```
$variable AS DataType <- DO {{imperative_nld_sentence}}
```

```
$flag AS Boolean <- DO {{check that $list items are in acsending order}}
$settFile AS File <- DO {{open settings file in read/write, binary mode}}
```

## Getting User Input (`INPUT:`)
When your algorithm needs data from the user during execution, use the `INPUT:` keyword. This keyword:
* this operation involves external interaction.
* waits until the user provides the requested input.
* is followed by a simple natural language description of what you expect the user to provide.
* The value entered by the user is returned by the statement and usually assigned to a variable.

#### Syntax
```
$variable AS DataType <- INPUT <nld_data_description>
```
In this syntax, `<nld_data_description>` is the Natural Language Description following `INPUT:`:
* **Role**: Its primary role is to describe the specific piece of data the user is expected to provide.
* **Form**: Typically a noun phrase or descriptive sentence fragment identifying the required input (e.g., "the user's age", "a valid email address", "the path to the input file", "'Y' to confirm, 'N' to cancel"). It is not an imperative command like with `DO:`.

#### Examples

```
$userName AS String
$userAge AS Integer
$nums AS List<Integer>


$userName <- INPUT: the user's full name    // Waits until the user inputs a string
$userAge <- INPUT: the user's age as an integer  // Waits until the user inputs an integer
$nums <- INPUT: get some Integers from user // Waits until the user inputs a list of integers
$keyPressed AS String <- INPUT any single character  // Waits until the user presses any key
```

## Notifying the User (`NOTIFY:`)
In our pseudocode dialect, we need a way to represent the algorithm communicating information outwards to the user. While traditional pseudocode often uses `PRINT`,  `OUTPUT`, or `DISPLAY` (usually implying console output), our dialect uses the more abstract `NOTIFY:` keyword.

The `NOTIFY:` statement signifies that the algorithm should make the user aware of something. It abstracts away the specific method of notification (e.g., printing to a terminal, showing a dialog box, updating a status bar, playing a sound, etc.). It focuses purely on the intent to inform.

#### Characteristics
* **User-Facing**: It's intended for information directed at the human user.
* **Non-Interactive**: It's a one-way communication. The algorithm presents the information and continues execution immediately; it does not wait for user acknowledgement or input. (If interaction is needed, use an `INPUT:` statement afterwards).
* **Abstract**: It describes what the user should be notified about, not how.
* **Severity Indication**: The description should include an indication of the notification's urgency or type (e.g., info, warning, error).

#### Syntax
```
NOTIFY: <nld_event_description> [as <severity_level>]
```

* **`NOTIFY`**:: The keyword indicating a user notification action.

* **`<nld_event_description>`**: The Natural Language Description following `NOTIFY:`.
   * **Role**: Its primary role is to describe the event, status, or piece of information the user should be made aware of. It explains what happened or what the current state is.
   * **Form**: A descriptive phrase or sentence. Can and often should include variable values to provide context.

* **`[as <severity_level>]`**: (Highly Recommended) Specifies the optional nature or urgency of the notification. Common levels include:
   * **info**: General informational message.
   * **success**: Confirmation of a successful operation.
   * **warning**: Indicates a potential issue or something the user should be cautious about.
   * **error**: Reports that an operation failed or an error occurred.
   * **critical**: Reports a severe error that might have significant consequences or require immediate attention.

When the pseudocode encounters a `NOTIFY:` statement, it means "At this point, the running program should present the described information to the user, classified by the given severity." The person implementing the pseudocode would then choose the most appropriate mechanism based on the description, the severity level, and the application's context (e.g., a GUI application might use a pop-up for critical, while a command-line tool might print in red text).

#### Examples

```
NOTIFY: the file processing is starting as info
// ... processing steps ...
NOTIFY: file processing completed successfully as success

// Implementation Hint: Could be simple console messages or status bar updates. Success notifications could be silent or a brief status message.
```

```
$calculationResult AS Real <- DO: perform complex calculation
NOTIFY: the final result is $calculationResult as info

// Implementation Hint: Could print to console, update a text field in a GUI.

```

```
$itemsLeft AS Integer <- DO: get items remaining in stock
IF $itemsLeft < 10 THEN
    NOTIFY: inventory level is low ($itemsLeft items remaining) as warning
ENDIF

// Implementation Hint: Might be a console message, a non-blocking notification pop-up, or changing an indicator icon's color.
```

```
$fileHandle AS File <- DO: attempt to open important_data.log for writing

IF $fileHandle IS NULL THEN // Assuming NULL indicates failure
    NOTIFY: failed to open important_data.log for writing as critical
    // Algorithm might terminate or take recovery steps here
ELSE
    NOTIFY: successfully opened important_data.log as success
    // ... proceed to write ...
    DO: close $fileHandle
ENDIF

// Implementation Hint: critical strongly suggests a very visible notification, perhaps a modal dialog
// (even though NOTIFY itself isn't blocking, the response to a critical notification might halt further
// progress), logging, or loud alerts.
```

#### Contrast with Other Keywords
* **`NOTIFY:` vs. `INPUT:`**: `NOTIFY:` sends information out non-interactively. `INPUT:` pauses and waits to receive information in.
* **`NOTIFY:` vs. `DO:`**: `NOTIFY:` is purely for communicating with the user. `DO:` is for describing internal operations or actions the algorithm performs (which might result in a value, assigned using `<- DO:`, or just perform an action). Writing to a log file intended only for developers/debugging would likely use `DO: write ... to log`, whereas `NOTIFY:` is for the end-user.

#### Best Practices:
* **Always Include Severity**: While optional in the syntax, always try to include `as <severity_level>`. It dramatically improves clarity and guides implementation.
* **Be Descriptive**: The natural language part should clearly convey the necessary information. Include relevant variable values.
* **Stay Abstract**: Avoid specifying how to notify (e.g., don't write `NOTIFY: show a blinking red dialog box...`). Let the severity and description guide the implementation.
* **Define Standard Levels (Optional)**: Consider establishing a standard set of severity levels for consistency within your project or team.

## Identation
Indentation visually represents the structure and nesting of your logic. It makes code drastically easier to read and understand by grouping related instructions. We use identation in two scenarios:

1.	Blocks
2.	Continuation

In this version of pseudocode, we use 3-space identation but it is NOT a holy number.

#### Indenting Inside Blocks
Any sequence of instructions that is logically contained within or controlled by another statement (forming a block) should be indented relative to that controlling statement. Every time you start a new block (inside `IF`, `WHILE`, `FUNCTION`, etc.), indent by exactly 3 spaces relative to the line that started the block. When a block ends (`ENDIF`, `ENDWHILE`, `ENDFUNCTION`, etc), the `END` keyword should align vertically with the keyword that started the block (`IF`, `WHILE`, `FUNCTION`, etc.).

```
// Level 0
FUNCTION CalculateGrade( $score AS INTEGER ) RETURNS STRING
   // Level 1 (Inside FUNCTION)
   $grade AS String

   IF $score >= 90 THEN
      // Level 2 (Inside IF)
      $grade <- "A"
   ELSE IF $score >= 80 THEN
      // Level 2 (Inside ELSE IF)
      $grade <- "B"
   ELSE
      // Level 2 (Inside ELSE)
      $grade <- "C" // Assuming C is lowest for simplicity
      // Still Level 2
      LogLowScore($score) // Another instruction in the ELSE block
   ENDIF // Aligned with IF/ELSE IF/ELSE

   // Level 1 (Back inside FUNCTION)
   RETURN $grade
ENDFUNCTION // Aligned with FUNCTION

// Level 0
$studentScore AS Integer
$finalGrade AS String
// Level 0
$studentScore <- 85
$finalGrade <- CalculateGrade($studentScore) // Function call is on level 0
OUTPUT $finalGrade

```

#### Indenting Line Continuations
When a single logical instruction or statement is too long to fit comfortably on one line, subsequent lines belonging to that same instruction should be indented. Every time you continue a long line onto the next, you should ident the continuation line or lines by 3 spaces.

```
$longVariableName <- CalculateValue($param1, $param2) +
   AnotherFunctionCall($param3, $param4) - $someOffset

IF ($conditionA AND $conditionB) OR
   ($conditionC AND $conditionD) THEN
   // Instructions inside IF...
ENDIF
```

# Basic Operations
## Equality
`=`

## Logical AND
`∧`

## Logical OR
`∨`

## Logical NOT
`¬`

# The Flow of Execution
In imperative programming, we give the computer a list of instructions, called **statements**, to execute. Understanding the flow of execution – the order in which these statements are run – is crucial to knowing how your program will behave.

#### Statements: The Building Blocks
Your pseudocode program is made up of statements. Each line or distinct instruction you write is typically a statement.

```
// This line defines a constant - it's a statement
CONST $MAX_SCORE <- 100

// This line assigns a value to a variable - it's a statement
$playerScore <- 0

// This line displays output - it's a statement
OUTPUT "Welcome!"
```

#### Default Flow: Sequential Execution
Imagine reading a recipe or a checklist. You start at the top and go down, step by step. By default, pseudocode works the same way. Execution starts at the very first statement of your program and proceeds **sequentially**, one statement after the next, from top to bottom.

```
// 1. Start here
DISPLAY "Step 1: Initializing."
// 2. Then execute this
$count <- 1
// 3. Then execute this
DISPLAY "Step 2: Count is " + $count
// 4. Program finishes (if this is the end)
```

The computer runs line 1, then line 2, then line 3, in that exact order.

#### Changing the Flow: Control Statements
Sequential flow is simple, but not very powerful. What if we want to make decisions or repeat actions? For this, we use **control flow statements**. This type of statements consist of sections called **clauses**.

#### Inside Control Statements: Internal Flow
Control flow statements have their own internal logic that determines how execution proceeds within them. At the program level, these control flow statements are executed after the previous and before the next statement but from the internal viewpoint the flow of execution is NOT sequential and is specific to them.

#### Summary
* Programs run **statements** one after another (**sequential flow**) by default.

* **Simple statements** (assignments, output, etc.) perform one action, then flow moves to the next statement.

* **Control flow statements** (IF, WHILE, etc.) alter the sequential flow.

* Control flow statements have **clauses** (conditions, blocks) and **internal logic** that decides which parts to execute and whether to repeat or skip, before flow continues to the statement after the control structure.

* Understanding this distinction between the overall sequential flow and the internal flow directed by control statements is key to writing effective pseudocode!

## IF-THEN

### Compactness
In the case of simple `IF` and `THEN` clauses, we can compact the whole statement in a single line.
```
// Finding the minimum & maximum in a user-provided list of integers...

$numbers AS List<Integer>
$numbers <- DO: get a list of Integers from user

IF ($numbers IS EMPTY) THEN
   NOTIFY the list is empty as error
   DO: exit with a suitable return code
ENDIF

$min AS Integer <- $numbers[0]
$max AS Integer <- $numbers[0]
FOR EACH $num IN $numbers DO
   IF $num < $min THEN $min <- $num ENDIF
   IF $num > $max THEN $max <- $num ENDIF
ENDFOR

OUTPUT "Max number: ", $max
OUTPUT "Min number: ", $min
```

## IF-THEN-ELSE

## FOR-EACH

## DO-WHILE

## KEEP-DOING

# Functions and Streams
In this chapter we are going to introduce ways to structure and reuse code by encapsulating logic into callable units. All involve defining a named block of code that can be called, potentially take inputs (**parameters**), and execute a sequence of steps. 

## Functions
Functions compute and **return** a single value based on inputs or perform actions, cause side effects, possibly modifying state.

#### Declaration Syntax
Functions are defined using the `FUNCTION` keyword, followed by a name, an argument block (`PARAMS`/`ENDPARAMS`), an optional return type declaration, the body of the routine, and finally `ENDFUNCTION`.
```
FUNCTION FunctionName [RETURNS ReturnType]
   PARAMS
      <param1>
      <param2>
      ...
      <paramN>
   ENDPARAMS
   // Body
ENDFUNCTION
```

* `FUNCTION`: Begins the definition of any routine (whether it acts as a mathematical-like function or a procedure).
* `FunctionName`: The identifier (name) you give to the routine. This name is used to call the routine later.
* `[RETURNS ReturnType]` (Optional clause): The key difference between a value-returning function and an action-performing procedure lies in the optional `RETURNS` clause:
   * If present, this clause specifies that the routine is a function designed to return a value.
   * If absent, the routine is a procedure designed primarily for its actions/side effects.
* `PARAMS`: The keyword that begins the parameters block where all the routine's parameters (arguments it accepts) are defined.
* `ENDPARAMS`: The keyword that marks the end of the parameter definition block.
* `// Body`: A placeholder representing the sequence of statements and logic that make up the routine's implementation. This is where computations happen, actions are performed.
* `ENDFUNCTION`: The keyword that marks the end of the entire function definition block.

#### Parameters syntax
One or more parameters can be declared using the following syntax. If function does not take any parameter, the whole `PARAMS`/`ENDPARAMS` block must be omitted.
```
[IN | OUT | INOUT] $argName AS DataType [<- <defaultValue>]
```
* `[IN | OUT | INOUT]`: Optional [Parameter Directionality](#parameter-directionality-in-out-inout)
* `$argName`: The identifier (name) used to refer to this parameter inside the body of the routine.
* `AS DataType` (mandatory): Declares the expected data type for the parameter.
* `[<- defaultValue]` (Optional default value):
   * If **present**, this argument becomes optional for the caller. If the caller doesn't provide a value/argument for this parameter, it will automatically be initialized with the `defaultValue`.
   * Arguments with default values must come after arguments without default values in the list.

### Procedures: no return value
A procedure is a routine that primarily performs actions or causes side effects (like printing to the screen, modifying a file, updating a global variable), or modifying an `OUT`/`INOUT` argument. It doesn't return a computed value back to the caller. To define a procedure, simply omit the `[RETURNS DataType]` part.

It is forbidden to return a value in a procedure using `RETURN $value` but it is possible to employ `RETURN` to exit the peocedure any time.

#### Example
```
// Example: Procedure to display a formatted message
FUNCTION ShowMessage
   PARAMS
      IN $message AS String         // Input: The text to show
      IN $prefix AS String <- "[INFO]" // Optional input: A prefix, defaults to "[INFO]"
   ENDPARAMS

   // Body: Performs the action
   OUTPUT $prefix + " " + $message
   // No RETURN statement needed (or use bare RETURN for early exit)

ENDFUNCTION // No RETURNS clause
```

### Functions with only one return value
A function is a routine whose primary purpose is to compute a value and return it to the caller. The name "function" and invocation syntax is inspired by mathematics.

To define a function, you must include the `RETURNS ReturnType` part, specifying the type of the value that will be returned. Inside the function's body, the flow of execution must always reach to a `RETURN` statement followed by the value (or an expression) of `ReturnType` in declaration to send the result back.

#### Example
```
FUNCTION AddNumbers RETURNS Integer // Specifies that an Integer value will be returned
   PARAMS
      $num1 AS Integer
      IN $num2 AS Integer
   ENDPARAMS

   // Body: Computes the value
   $sum AS Integer
   $sum <- $num1 + $num2
   RETURN $sum // Returning the computed value

ENDFUNCTION
```

### Functions with multiple return values
If you need to return multiple values, you typically return them packaged in a structured type like a `Tuple`, `RECORD`, or `List`.

```
// Example: Function returning both min and max of a list
FUNCTION FindMinMax RETURNS Tuple<Integer, Integer>  // Specifies returning a pair
   ARGS
      IN $numbers AS List<Integer> // Input: List to search
   ENDARGS

   // Assume list is not empty for simplicity
   $min AS Integer <- $numbers[0]
   $max AS Integer <- $numbers[0]
   FOR EACH $num IN $numbers DO
      IF $num < $min THEN $min <- $num ENDIF
      IF $num > $max THEN $max <- $num ENDIF
   ENDFOR

   RETURN ($min, $max) // Returning a 2-tuple (pair)...
ENDFUNCTION
```

### Default-Valued Parameters
In the function definition, you can make a parameter optional by providing a default value using the `<- defaultValue` syntax.

```
FUNCTION Connect
   PARAMS
      IN $host AS String           // Required parameter
      IN $port AS Integer <- 8080  // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION
```

* If the caller does not provide an argument corresponding to `$port` or `$timeout` when calling Connect, the function will automatically use `8080` for `$port` and `5000` for `$timeout` internally.
* Parameters with default values must be listed after parameters without default values in the `PARAMS` block for clarity, especially when using positional arguments.

### Function Invocation
The basics of function invocation syntax is inspired by mathematics. To call a function; we must put a comma-separated list of literals, variables, or anything that is evaluated to a value; in a pair of paranthesis just in front of function name.

#### Example
```
FUNCTION Connect
   PARAMS
      IN $host AS String           // Required parameter
      IN $port AS Integer <- 8080  // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION

Connect("example.com")
Connect("example.com", 443)
Connect("example.com", 443, 1000)
```

#### Parameters vs. Arguments
These terms are related but distinct:
* **Parameter**: A variable declared within the `PARAMS`/`ENDPARAMS` block of a function definition. It's a placeholder defining the name, expected data type, and directionality (`IN`, `OUT`, `INOUT`) of an expected input or output. In the above example `$host`, `$port`, and `$timeout` are parameters.
* **Argument**: The actual value (literal or evaluation of an expression) or variable supplied by the caller when the function is invoked. Arguments provide the concrete data that the parameters will hold during the function's execution. In the above example `"example.com"`, `443`, and `1000` are arguments.

There are three methods for correspondence between arguments and methods:
* [Calling functions using positional arguments](#calling-functions-using-positional-arguments)
* [Calling functions using named arguments](#calling-functions-using-named-arguments)
* [Calling functions using both positional and named arguments (mixed call)](#calling-functions-using-both-positional-and-named-arguments-mixed-call)

### Calling Functions using Positional Arguments
This is the simplest invocation method, mirroring mathematical notation. Arguments are matched to parameters based strictly on their order. The first argument in the call corresponds to the first parameter in the definition, the second argument to the second parameter, and so on.

#### Syntax
```
DoSomething(arg1, arg2, ...)
```

* Concise
* familiar
* Requires remembering the exact parameter order.
* It can become unclear with many parameters or boolean flags.
* It is difficult to skip optional arguments unless they are the last ones.

#### Examples
```
FUNCTION Calculate RETURNS Integer
   PARAMS
      IN $a AS Integer
      IN $b AS Integer
      IN $op AS String <- "+"
   ENDPARAMS

   // Body
ENDFUNCTION

$result AS Integer
$result <- Calculate(10, 5)      // $a=10, $b=5, $op uses default "+"
$result <- Calculate(20, 7, "*") // $a=20, $b=7, $op="*"
```

```
FUNCTION UpdateStatus
   PARAMS
      OUT $msg AS String
      IN $code AS Integer
   ENDPARAMS

   // Updating $msg in here...
ENDFUNCTION

$statusMessage AS String
UpdateStatus($statusMessage, 0) // $statusMessage is the variable argument for $msg, 0 for $code
                                // $statusMessage will be updated by the function
```

###  Calling Functions using Named Arguments
This method explicitly matches arguments to parameters by name, making calls clearer and more robust. Arguments are specified using the parameter name (including the `$`), followed by assignment operator (`<-`), and then the value/variable. The order in which named arguments are listed does not matter.

#### Syntax

```
DoSomething($param1Name <- value1, $param2Name <- value2, ...)
```

* Highly readable and self-documenting
* Order independence prevents errors
* Easy to specify only certain optional arguments regardless of their position
* More verbose than positional arguments.

#### Examples

```
FUNCTION Calculate RETURNS Integer
   PARAMS
      IN $a AS Integer
      IN $b AS Integer
      IN $op AS String <- "+"
   ENDPARAMS

   // Body
ENDFUNCTION

$result AS Integer
$result <- Calculate($a <- 10, $b <- 5)              // $op uses default "+"
$result <- Calculate($b <- 7, $a <- 20, $op <- "*")  // Order doesn't matter
$result <- Calculate($a <- 15, $op <- "/", $b <- 3)  // Order doesn't matter
```

```
FUNCTION Connect
   PARAMS
      IN $host AS String             // Required parameter
      IN $port AS Integer <- 8080    // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION

Connect($host <- "api.server.net") // $port and $timeout use defaults
Connect($host <- "db.server.local", $timeout <- 10000) // $port uses default
Connect($port <- 9001, $host <- "backup.server")     // $timeout uses default, order doesn't matter
```

### Calling Functions using Both Positional and Named Arguments (Mixed Call)

You can combine both styles in a single function call for flexibility, that is provide some initial arguments positionally, then switch to named arguments for the remainder.

**Positional-before-named rule**: All positional arguments **MUST** come **BEFORE** all named arguments. You cannot place a positional argument after a named argument in the same call.

#### Syntax

```
DoSomething(posArg1, posArg2, $namedArgN <- valueN, $namedArgM <- valueM, ...)
```

* Offers flexibility: uses concise positional style for required initial parameters and clear named style for optional or later parameters.
* Requires adherence to the **positional-before-named rule**.

#### Example

```
FUNCTION Connect
   PARAMS
      IN $host AS String             // Required parameter
      IN $port AS Integer <- 8080    // Optional parameter, defaults to 8080
      IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
   ENDPARAMS
   // ... function body ...
ENDFUNCTION

// VALID Mixed Calls:
Connect("main.server.com", $timeout <- 15000)
// $host gets "main.server.com" (positional)
// $port uses default 8080 (skipped positionally, not specified by name)
// $timeout gets 15000 (named)

Connect("backup.server", $port <- 8081, $timeout <- 2000)
// $host gets "backup.server" (positional)
// $port gets 8081 (named)
// $timeout gets 2000 (named)

// INVALID Mixed Call:
// Connect($port <- 9000, "fails.com") // ERROR: Positional argument after named argument
```

### Parameter Directionality (`IN`, `OUT`, `INOUT`)
When defining an argument within the `PARAMS`/`ENDPARAMS` block, you can specify its **Parameter Directionality**. This is an optional keyword (`IN`, `OUT`, or `INOUT`) that declares the intended direction of data flow for that parameter between the caller's argument and the routine's parameter. It defaults to `IN`.

* `IN` Directionality:
   * Data is conceptually transferred from the caller's argument to the routine's parameter at the start of the call.
   * Any change made to the parameter, remains inside of the routine and is NOT reflected back to the caller.
   * The caller's argument can be a literal, a variable, an expresison, or anything else that is evaluated to a value.
   * Use cases include passing values needed for computation, configuration settings, or data to be read without modification.

   ```
   PARAMS
      IN $config1 AS String // Input configuration #1
      $config2 AS String // Input configuration #2
   ENDPARAMS
   ```

* `OUT` Directionality:
   * Data is conceptually transferred from routine's parameter to the the caller's argument at the end of the call.
   * The caller's argument must only be a variable and the initial value of the caller's variable is generally disregarded; the routine is responsible for assigning the output value.
   * Use cases include returning secondary results, status indicators, or outputs when a primary `RETURN $value` is not used or insufficient.

   ```
   PARAMS
      OUT $statusMessage AS String // Output status description
      OUT $resultCode AS Integer  // Output numerical result code
   ENDPARAMS
   ```

* `INOUT` Directionality:
   *  Data is initially transferred from the caller's argument to the routine's parameter. The routine can then read this value and modify it. Upon completion, the final value of the parameter is transferred back to the caller's argument.
   * The caller's argument must only be a variable.
   * Use cases include modifying data structures passed by the caller (e.g., sorting a list in place), updating counters or state variables provided by the caller.

   ```
   PARAMS
      INOUT $dataList AS List<Person> // Input data, potentially modified by routine
   ENDPARAMS
   ```

#### Summary
| Mode | Purpose | Data Flow Direction | Initial Value Usage | Caller Variable Effect |
|------|---------|---------------------|---------------------|-----------------| 
| IN |Input Only | Caller -> Routine | Read by Routine | Not Affected |
|OUT | Output Only | Routine -> Caller | Ignored by Routine | Updated on Exit |
| INOUT | Input and Output | Caller <-> Routine | Read by Routine | Updated on Exit |

## Streams

Beyond regular functions that compute a single result or perform a distinct action, AlgoDraft supports **Streams**. Streams are used to define routines that can **yield** a sequence of values over time based on initial configuration or internal logic, producing each value only when requested (lazy generation).

#### Syntax

The definition structure of streams shares similarities with function definitions regarding the overall block structure and parameter list.

```
STREAM StreamName YIELDS YieldType // MANDATORY: Declares the type of items yielded
   PARAMS
      $param1 AS DataType1       // Parameter definitions
      IN $param2 AS DataType2 <- defaultValue
      // ... Only IN parameters are allowed for Streams ...
   ENDPARAMS

   // Body: Contains logic, local variables, and YIELD statements
   // ... Code executes incrementally ...

   YIELD valueOfTypeYieldType // Produces the next value and pauses

   // ... More code ...

   RAISE EXHAUSTION // Optional: Manually terminate the stream

   // ... More code ...

   // Reaching ENDSTREAM or a bare RETURN also terminates the stream

ENDSTREAM
```

#### Note

Only `IN` [Parameter Directionality](#parameter-directionality-in-out-inout) is permitted for parameters defined within a stream's `PARAMS` block. The multi-stage, suspend/resume nature of streams makes the semantics of `OUT` and `INOUT` parameters (which typically relate to the single completion state of a routine) complex and counter-intuitive in this context. Inputs configure the stream's generation process; outputs are exclusively handled via `YIELD`.

#### Semantics: Lazy Evaluation and State

The behavior of streams differs significantly from regular functions:

1. **Stream Object Creation**: Calling a stream definition (e.g., `myStream <- StreamName(arg1, ...)`) does not execute the code inside the `STREAM` body immediately. Instead, it creates and returns a special [`Stream` object](#stream-data-type). This object represents the paused, ready-to-run generator and encapsulates its internal state.
2. **Lazy Execution**: The code inside the stream body only starts running when the first request for a value is made from its `Stream` object.
3. **`YIELD` Keyword**: When the `YIELD $value` statement is encountered inside the stream's body:
   * The stream's execution is paused immediately after the `YIELD`.
   * The specified value (which must match the `YieldType`) is sent back as the result of the request that resumed the stream.
   * The stream's internal state is preserved.
4. **Resumption**: The next request for a value will resume the stream's execution right after the `YIELD` statement where it last paused, with its preserved state intact.
5. **Exhaustion**: The stream becomes "exhausted" (finished) when:
   * It reaches the `ENDSTREAM` keyword naturally.
   * A bare `RETURN` statement is executed within its body.
   * A `RAISE EXHAUSTION` statement is explicitly executed.






# Data Structures

## Understanding Data Structures in AlgoDraft

In AlgoDraft, data structures are fundamental ways to organize, manage, and store data effectively. They are built using basic data types (like `Integer`, `String`, `Boolean`) and provide a blueprint for how data relates and how it can be accessed or manipulated.

To better understand and choose the right data structure for a task, we categorize them based on several key properties or characteristics:

* **Homogeneity**: Does the structure typically hold elements of the same data type (**homogeneous**) or can it hold elements of different types (**heterogeneous**)? [Generics](#generics) should be used to emphasize homogeneity `$list <- CREATE List<Integer>`.
* **Sequentiality**: Do the elements have a defined order or follow a specific processing sequence (like First-In-First-Out)? Can you predictably move from one element to the next logical one in a sequence? (Yes/No). Structures where elements don't have a defined positional or processing order are **non-sequential**.
* **Linearity**: Do the elements form a single, non-branching sequence? (Yes/No). In a linear structure, each element (except ends) connects to at most one predecessor and one successor. Trees and Graphs are examples of non-linear structures. All linear structures are inherently sequential.
* **Mutability**: Can the structure be changed after creation? Can elements be added, removed, or modified (Mutable)? Or is the structure fixed once created (Immutable)?
* **Iterability**: Is it possible to systematically visit or process each element contained within the structure (e.g., using a `FOR EACH` loop)? (Yes/No). This applies even to non-linear structures, though the traversal method might be more complex (like Depth-First or Breadth-First). Structures like Records, which group named fields, are typically not considered iterable in this sense (you access fields by name, not by iterating over values as members of a collection).
* **Uniqueness**: Does the structure enforce uniqueness for its elements or in some way? (e.g., sets contain unique elements, mappings have unique keys).

#### **AlgoDraft Data Structure Characteristics**

| Data Structure | Sequentiality                 | Linearity        | Homogeneity                   | Mutability                                             | Iterability | Uniqueness                                         | Primary Use Case                                     |
| :------------- | :---------------------------- | :--------------- | :---------------------------- | :----------------------------------------------------- | :---------- | :------------------------------------------------- | :--------------------------------------------------- |
| **Tuple**      | Yes                           | Yes              | **Not Enforced²**   | Immutable                                              | Yes         | Duplicates Allowed                                 | Grouping a fixed number of related, ordered items.   |
| **String**     | Yes (Sequence of chars)       | Yes              | **Yes (Characters)**          | **Immutable**                                          | Yes         | Duplicates Allowed (Characters)                    | Representing textual data.                           |
| **List**       | Yes                           | Yes              | **Not Enforced²**             | Mutable                                                | Yes         | Duplicates Allowed                                 | Storing sequences where order matters, random access.|
| **Set**        | No                            | No               | **Not Enforced²**             | Mutable                                                | Yes         | **Elements Unique**                                | Storing unique items, checking membership quickly.   |
| **Mapping**    | **Not Enforced³**             | No               | **Not Enforced²** (Values)    | Mutable                                                | Yes⁴        | **Keys Unique**, Values can duplicate              | Associating unique keys with arbitrary values.       |
| **Stack**      | Yes (LIFO - Last-In First-Out)| Yes              | **Not Enforced²**             | Mutable                                                | Yes⁵        | Duplicates Allowed                                 | Processing items in reverse order of arrival.        |
| **Queue**      | Yes (FIFO - First-In First-Out)| Yes              | **Not Enforced²**             | Mutable                                                | Yes⁵        | Duplicates Allowed                                 | Processing items in order of arrival.                |
| **Record**     | No (Fields have names)        | No               | **Not Enforced²**     | Fields names/number fixed; field values often mutable⁶ | No⁷         | **Field Names Unique**, Values can duplicate       | Grouping related data under specific names.          |
| **Tree**       | No (Hierarchical)             | **No (Hierarchical)** | **Not Enforced²** (Node Data) | Varies (Structure/Nodes can be mutable)                | Yes⁸        | Varies (Depends on specific tree type/rules)       | Representing hierarchical relationships.             |
| **Graph**      | No (Network)                  | **No (Network)** | **Not Enforced²** (Node/Edge Data) | Varies (Structure/Nodes/Edges can be mutable)        | Yes⁸        | Varies (Nodes/Edges might be unique depending on use)| Representing complex relationships and networks.     |

**Footnotes:**

² **Homogeneity:** For structures marked `Not Enforced`, homogeneity is not strictly required by AlgoDraft. Instances can contain elements of a single type (homogeneous) or mixed types (heterogeneous), depending on usage. This contrasts with `String`.

³ **Sequentiality:** For Mappings, sequentiality (order of keys/elements during iteration) is `Not Enforced` in AlgoDraft. While some underlying implementations might preserve order (e.g., insertion order), it should not be relied upon by default in algorithms unless explicitly stated for a specific AlgoDraft Mapping type.

⁴ **Iterability (Mapping):** Mappings are iterable, typically allowing iteration over keys, values, or key-value pairs. The iteration order is subject to footnote 3.

⁵ **Iterability (Stack/Queue):** Stacks and Queues are iterable, though primary interaction is often via `Push/Pop` or `Enqueue/Dequeue`. Iteration usually reflects the processing order (LIFO for Stacks, FIFO for Queues).

⁶ **Mutability (Record):** The *structure* of a Record (its fields and their names) is fixed after declaration, but the *values* held in those fields are typically mutable unless the field type itself is immutable (like a String or Tuple in AlgoDraft).

⁷ **Iterability (Record):** Records are not typically iterated over like collections. You access fields by name (`myRecord.fieldName`). Mechanisms might exist to reflect on fields, but that's different from iterating over contained elements.

⁸ **Iterability (Tree/Graph):** Trees and Graphs are iterable using specific traversal algorithms (e.g., Depth-First Search (DFS), Breadth-First Search (BFS)) which define the order elements (nodes/edges) are visited.


#### **Why This Matters in AlgoDraft**

Understanding these categories helps you write better pseudocode:

*   **Iterability:** Determines if you can use constructs like `FOR EACH element IN structure`.
*   **Sequentiality/Linearity:** Influences how you think about accessing elements (e.g., by index `list[i]`, by position `stack.Pop()`, or by relationships `node.LeftChild`).
*   **Mutability:** Tells you whether you can modify the structure in place or if operations create new structures.
*   **Non-Iterable Structures (like Record):** Require different access patterns, typically using dot notation (`record.field`).

By choosing a data structure whose properties match your needs, you can express your algorithms more clearly and efficiently in AlgoDraft.

## Object Instantiation
```
$obj AS DataType <- DataType()
$obj AS {{NLD describing the type}} <- DO {{create instance of the type}}
```

## Tuples

## String

### Substrings
To specify a substring out of a string variable, we use an interval (integer stream) just after the variable:
* `$myString[start .. end]`: Inclusive start, inclusive end
* `$myString[start .. end)`: Inclusive start, exclusive end
* `$myString(start .. end]`: exclusive start, inclusive end
* `$myString(start .. end)`: exclusive start, exclusive end

#### Example
```
$myString AS STRING <- "Example"

// "xamp"
OUTPUT $myString[1 .. 4]

// "xam"
OUTPUT $myString[1 .. 4)

// "am"
OUTPUT $myString(1 .. 4)

// "amp"
OUTPUT $myString(1 .. 4]
```

## Sets
Sets are mutable, iterable, non-sequential collection of unique elements.

#### Constructing
```
// Defining a set using literal notation...
$set1 AS Set<Integer> <- {1, 5, 2, 5} // Becomes {1, 2, 5}
$set2 AS Set<Integer OR String> <- {"apple", 3, "apple",} // Becomes {3, "apple",}
$set3 AS Set <- {1, "hello", TRUE, 1} // Becomes {1, "hello", TRUE}

// Defining an empty set...
$empty1 AS Set<SomeType> <- CREATE AN EMPTY Set<SomeType>
$empty2 AS Set<SomeType> <- CREATE Set<SomeType>
$empty3 AS Set<SomeType> <- ∅
```

**Notes**
* In set literals, trailing comma is allowed and it is a good practice to adding values later easily.
* `AN EMPTY` although verbose, it is a good practice to emphasize the emptiness of the newly created set.

#### Membership/Containment Operators
These binary operators perform a check, returning a Boolean value, to determine if the left operand is considered (IN, ∈) or is not considered (NOT IN, ∉) a member (an element) of the right operand, based on mathematical equality. These operators are typically used within conditional statements to control algorithmic flow based on an element's presence or absence in a set.

```
$numbers AS Set<Integer> <- {10, 20, 30}

IF 30 IN $numbers THEN
   NOTIFY {{30 is found}}
ENDIF

IF 20 ∈ $numbers THEN
   NOTIFY {{20 is found}}
ENDIF

IF 40 NOT IN $numbers THEN
   NOTIFY {{40 is missing}}
ENDIF

IF 50 ∉ $numbers THEN
   NOTIFY {{50 is missing}}
ENDIF

```

#### Equality/Inequality Operators

These binary operators perform a check, returning a Boolean value, to determine if two sets are considered equal or unequal. Set equality in AlgoDraft, following mathematical convention, means that both sets contain exactly the same elements, irrespective of the order in which elements were added or are internally stored.

AlgoDraft provides multiple syntaxes for expressing equality and inequality:
* **Equality**:
	* `=` (Symbolic, mathematical standard)
	* `EQUALS` (Keyword, for enhanced readability)
* **Inequality**:
	* != (Symbolic, common programming standard)
	* `≠` (Symbolic, mathematical standard)
	* `NOT EQUALS` (Keyword phrase)
	* `NOT ($setA EQUALS $setB)` (Negation of the keyword equality check)

While multiple forms exist for flexibility and accommodating different notation preferences, `=`and `≠` directly reflect standard mathematical notation, while **!=** is common in programming. `EQUALS` enhances readability, though `NOT EQUALS` is less conventional as a direct operator phrase compared to negating an equality check or using a dedicated inequality symbol.

```
// Define some sets for comparison using AlgoDraft syntax
$set1 AS Set<Integer> <- {10, 20, 30}
$set2 AS Set<Integer> <- {30, 10, 20} // Same elements, different literal order
$set3 AS Set<Integer> <- {10, 20, 40} // Different elements
$set4 AS Set<Integer> <- {10, 20}    // Different elements (subset)

NOTIFY {{Comparing set1 = {10, 20, 30}, set2 = {30, 10, 20}, set3 = {10, 20, 40}, set4 = {10, 20}.}}

// --- Equality Checks ---
IF $set1 = $set2 THEN
    NOTIFY {{$set1 is equal to $set2 using the = operator.}} // Expected
ELSE
    NOTIFY {{$set1 is not equal to $set2 using the = operator.}}
ENDIF

IF $set1 = $set3 THEN
    NOTIFY {{$set1 is equal to $set3 using the = operator.}}
ELSE
    NOTIFY {{$set1 is not equal to $set3 using the = operator.}} // Expected
ENDIF

IF $set1 EQUALS $set2 THEN
    NOTIFY {{$set1 is equal to $set2 using the EQUALS keyword.}} // Expected
ELSE
    NOTIFY {{$set1 is not equal to $set2 using the EQUALS keyword.}}
ENDIF

IF $set1 EQUALS $set4 THEN
    NOTIFY {{$set1 is equal to $set4 using the EQUALS keyword.}}
ELSE
    NOTIFY {{$set1 is not equal to $set4 using the EQUALS keyword.}} // Expected
ENDIF

// --- Inequality Checks ---
IF $set1 != $set3 THEN
    NOTIFY {{$set1 is not equal to $set3 using the != operator.}} // Expected
ELSE
    NOTIFY {{$set1 is equal to $set3 using the != operator.}}
ENDIF

IF $set1 != $set2 THEN
    NOTIFY {{$set1 is not equal to $set2 using the != operator.}}
ELSE
    NOTIFY {{$set1 is equal to $set2 using the != operator.}} // Expected
ENDIF

IF $set1 ≠ $set3 THEN
    NOTIFY {{$set1 is not equal to $set3 using the ≠ symbol.}} // Expected
ELSE
    NOTIFY {{$set1 is equal to $set3 using the ≠ symbol.}}
ENDIF

IF $set1 ≠ $set2 THEN
    NOTIFY {{$set1 is not equal to $set2 using the ≠ symbol.}}
ELSE
    NOTIFY {{$set1 is equal to $set2 using the ≠ symbol.}} // Expected
ENDIF

IF $set1 NOT EQUALS $set3 THEN
    NOTIFY {{$set1 is not equal to $set3 using the NOT EQUALS keywords.}} // Expected
ELSE
    NOTIFY {{$set1 is equal to $set3 using the NOT EQUALS keywords.}}
ENDIF

IF $set1 NOT EQUALS $set2 THEN
    NOTIFY {{$set1 is not equal to $set2 using the NOT EQUALS keywords.}}
ELSE
    NOTIFY {{$set1 is equal to $set2 using the NOT EQUALS keywords.}} // Expected
ENDIF

// --- Negated Equality Check ---
IF NOT ($set1 EQUALS $set3) THEN
    NOTIFY {{The negation of ($set1 EQUALS $set3) is true.}} // Expected
ELSE
    NOTIFY {{The negation of ($set1 EQUALS $set3) is false.}}
ENDIF

IF NOT ($set1 EQUALS $set2) THEN
    NOTIFY {{The negation of ($set1 EQUALS $set2) is true.}}
ELSE
    NOTIFY {{The negation of ($set1 EQUALS $set2) is false.}} // Expected
ENDIF
```

#### Emptiness Operators

These operators perform a check, returning a Boolean value, to determine if a set is considered empty `(IS EMPTY`, `= ∅`) or not empty (`IS NOT EMPTY`, `≠ ∅`). The keyword forms (`IS EMPTY,` `IS NOT EMPTY`) act as direct unary tests of the set's state, while the symbolic forms (`= ∅,` `≠ ∅`) act as binary comparisons between the set variable operand and the empty set (`∅`) value/literal.

```
$setA <- ∅   // Or $setA <- CREATE Set<Integer>()
$setB AS Set<Integer> <- {1, 2, 3,}

IF $setA IS EMPTY THEN
   NOTIFY {{$setA is empty}}
ENDIF

IF $setA = ∅ THEN
   NOTIFY {{$setA is empty}}
ENDIF

IF $setB IS NOT EMPTY THEN
   NOTIFY {{$setB is not empty}}
ENDIF

IF $setB ≠ ∅ THEN
   NOTIFY {{$setB is not empty}}
ENDIF
```

## Queue

### Methods

#### Enqueue

#### Dequeue

# User-Defined Types



# Intermediate Syntax

## Error handling
The `TRY-CATCH-ELSE-FINALLY` statement allows you to gracefully handle errors (exceptions) that might occur during the execution of a specific block of code. It prevents your program/algorithm from crashing unexpectedly and lets you define specific recovery actions, logging, or cleanup procedures.

#### Syntax
```
TRY
   // Block 0: Code that might cause an error.
   // This is the code you want to "protect".
   <block_0: potentially problematic instructions>

CATCH <error1> [AS $errorInfo1] // Optional: Catch a specific error type
   // Block 1: Executes ONLY if ErrorType1 occurred in Block 0.
   <block_1: handling instructions for ErrorType1>

CATCH <error2> [AS $errorInfo2] // Optional: Catch another specific error
   // Block 2: Executes ONLY if ErrorType2 occurred in Block 0.
   <block_2: handling instructions for ErrorType2>

CATCH // Optional: Generic catch-all (use sparingly, usually last)
   // Block 3: Executes ONLY if an error occurred that wasn't caught above.
   <block_3: handling instructions for any other error>

ELSE // Optional: Executes ONLY if NO error occurred in Block 0.
   // Block 4: Code that should run only upon successful completion of Block 0.
   <block_4: success-path instructions>

FINALLY // Optional: Always executes, regardless of errors or success.
   // Block 5: Cleanup code (e.g., closing files, releasing resources).
   <block_5: cleanup instructions>

ENDTRY // Marks the end of the error handling structure.
```

#### Caluses and Keywords
* `TRY`: Identifies the block of code under observation for errors.
* `CATCH <error> [AS $errorInfo]` (specific): Defines a handler for a specific type of error. This can be an error type (like `FileNotFoundError`) or an NLD describing the error. If that error occurs in the `TRY` block, the corresponding `CATCH` block executes. The optional `AS $errorInfo` part lets you capture details about the error into a variable (if needed). You can have multiple specific `CATCH` blocks.
* `CATCH` (Generic): Catches any error not handled by preceding specific `CATCH` blocks.
* `ELSE`: Defines a block that runs only if the `TRY` block completes successfully without raising any errors.
* `FINALLY`: Defines a block that always runs after the `TRY` block and any executed `CATCH` or `ELSE` block. It runs whether an error occurred or not, whether it was caught or not. Essential for cleanup.
* `ENDTRY`: Closes the structure.

#### The Flow of Execution
Code inside the `TRY` block executes in its usual flow of execution. There are three possibilities:
1.	If no error occurs:
      * The entire `TRY` block completes successfully.
      * All `CATCH` blocks are SKIPPED.
      * The `ELSE` block executes (if present).
      * The `FINALLY` block executes (if present).
      * Execution continues after `ENDTRY`.
2.	If an error occurs:
      * The `TRY` block stops immediately at the point of error.
      * The system looks for a matching `CATCH` block from more specific, to more generic, the writing order is not respected.
      * If a matching `CATCH` is found, its code executes.
      * The `ELSE` block is SKIPPED.
      * The `FINALLY` block executes (if present).
      * Execution continues after `ENDTRY`.
3.	If NO matching `CATCH` is found, the error might propagate outwards (or crash the program, depending on higher-level handling). The `FINALLY` block still runs.


#### Examples

```
$pthLog <- DO: read the the log file path from settings

TRY
   $log AS File OR NULL <- NULL
   $LOG <- DO: open $pthLog in read/write, text mode
CATCH {{file does not exist}}
   NOTIFY {{the log path does not exist}}
CATCH {{some other file system error}}
   NOTIFY {{the probability of issues in the file system}}
CATCH
   NOTIFY {{an error occurred}}
FINALLY
   IF $log IS NULL THEN
      DO {{exit the program with a suitable return code}}
   ENDIF 
ENDTRY
```








# Appendix 1: Unicode Characters

|**Glyph**|**Unicode name**|**Code**|**HTML Entity**<br>**(Numeric)**|**HTML Entity**<br>**(Name)**|**Comments**|
|------|----------|-----------|----------|-----------|--------------|
|`✍`|WRITING HAND|`U+270D`|`&#9997;`<br>`&#x270D;`|||
|`≠`|NOT EQUAL TO|`U+2260`|`&#8800;`<br>`&#x2260;`|`&ne;`||
|`∧`|LOGICAL AND|`U+2227`|`&#8743`<br>`&#x2227`|`&and;`||
|`∨`|LOGICAL OR|`U+2228`|`&#8744;`<br>`&#x2228;`|`&or;`||
|`¬`|NOT SIGN|`U+00AC`|`&#172;`<br>`&#xac;`|`&not;`||
|`Δ`|GREEK CAPITAL LETTER DELTA|`U+0394`|`&#916;`<br>`&#x394;`|`&Delta;`||
|`∈`|ELEMENT OF|`U+2208`|`&#8712;`<br>`&#x2208;`|`&Element;`<br>`&in;`<br>`&isin;`<br>`&isinv;`||
|`∉`|NOT AN ELEMENT OF|`U+2209`|`&#8713;`<br>`&#x2209;`|`&NotElement;`<br>`&notin;`<br>`&notinva;`||
|`∅`|EMPTY SET|`U+2205`|`&#8709;`<br>`&#x2205;`|`&empty;`||
|**⊂**|SUBSET OF|`U+2282`|`&#8834;`<br>`&#x2282;`|`&sub;`||
|**⊃**|SUPERSET OF|`U+2283`|`&#8835;`<br>`&#x2283;`|`&sup;`||
|`⊄`|NOT A SUBSET OF|`U+2284`|`&#8836;`<br>`&#x2284;`|`&nsub;`||
|`⊅`|NOT A SUPERSET OF|`U+2285`|`&#8837;`<br>`&#x2285;`|`&nsup;`||
|`⊆`|SUBSET OF OR EQUAL TO|`U+2286`|`&#8838;`<br>`&#x2286;`|`&sube;`<br>`&SubsetEqual;`<br>`&subseteq;`||
|`⊇`|SUPERSET OF OR EQUAL TO|`U+2287`|`&#8839;`<br>`&#x2287;`|`&SupersetEqual;`<br>`&supe;`<br>`&supseteq;`||
|`⊈`|NEITHER A SUBSET OF NOR EQUAL TO|`U+2288`|`&#8840;`<br>`&#x2288;`|`&NotSubsetEqual;`<br>`&nsube;`<br>`&nsubseteq;`||
|`⊉`|NEITHER A SUPERSET OF NOR EQUAL TO|`U+2289`|`&#8841;`<br>`&#x2289;`|`&NotSupersetEqual;`<br>`&nsupe;`<br>`&nsupseteq;`||
|**∪**|Union|U+222A|&#8746;<br>&#x222A;<br>&cup;|||
|**∩**|Intersection|U+2229|&#8745;<br>&#x2229;<br>&cap;|||
|||||||
|||||||

# Appendix 2: Naming conventions
##  camelCase
#### Rule
The first word starts with a lowercase letter. Every new word after that starts with an uppercase letter. There are no spaces or underscores between words.

#### Analogy
Imagine the humps of a camel 🐪 – the uppercase letters are like the humps after the initial lowercase start.

#### Examples

* `myVariableName`

* `calculateTotalPrice`

* `userName`

* `loopCounter`

#### Common Uses
Often used for variables and functions in languages like Java, JavaScript, C#, C++, and Swift.

## PascalCase or UpperCamelCase
#### Rule
Similar to camelCase, but the very first word also starts with an uppercase letter. Every word starts with uppercase. No spaces or underscores.

#### Analogy
Think of it as a "taller" camelCase where even the beginning is capitalized, like the start of a proper noun or a title.

#### Examples

* `MyVariableName`

* `CalculateTotalPrice`

* `UserName`

* `MainNavigation`

#### Common Uses
Very commonly used for naming Classes, Interfaces, Enums, and sometimes Types or Components in languages like Java, C#, JavaScript (especially for classes/components), Swift, and Pascal (where it got its name!).

## snake_case
#### Rule
All words are in lowercase. Words are separated by an underscore (_).

#### Analogy
Imagine a snake 🐍 slithering along the ground (all lowercase) with little bumps (the underscores).

#### Examples

* my_variable_name

* calculate_total_price

* user_name

* loop_counter

#### Common Uses
Very popular in Python for variables and functions. Also common in Ruby, Rust, and often used for database table and column names in SQL.

## SCREAMING_SNAKE_CASE, UPPER_SNAKE_CASE or MACRO_CASE
#### Rule
All words are in uppercase. Words are separated by an underscore (_).

#### Analogy
Like snake_case, but SHOUTING!

#### Examples

* MAX_CONNECTIONS

* PI_VALUE

* DEFAULT_TIMEOUT

* API_KEY

#### Common Uses
Almost universally used for naming Constants – values that are set once and never change during the program's run. You'll see this across many languages (Python, Java, JavaScript, C++, PHP, etc.).

## kebab-case
#### Rule
All words are in lowercase. Words are separated by a hyphen (-).

#### Analogy
Imagine words skewered like pieces on a kebab stick (the hyphen).

#### Examples
* my-variable-name

* calculate-total-price

* user-name

* main-navigation

#### Common Uses
You won't usually see this for variable or function names inside most common programming languages like Python, Java, or JavaScript, because the hyphen (-) is often used for subtraction. However, it's very common in:

* Web Development: CSS class names (.main-content), HTML attributes (data-user-id), URL slugs (/my-blog-post)

* Configuration files

* Sometimes in Lisp-family languages.

