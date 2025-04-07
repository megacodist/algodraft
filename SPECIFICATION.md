# Introduction
Humans need to convey their ideas or exchange ideas with other people. If you want to talk about mathematics, you need to know algebra, set theory, their theoroms, gargons among other things. Pseudocode is the language of algorithms.

It is the general way to express algorithmic ideas.

It is worth mentioning that there is NOT a standard pseudocode that everyone agrees upon.

AlgoDraft is the result of our effort to have the most formal-informal algorithm language: the most formal among all casual contracts and the most informal among all programming langiages.

## Characteristics
Pseudocode ia all about logic not implementation.

It must be as higher level as possible if we do not want to say the highest level. The ligh level in programming sense means free from any limiting technology, assuming everything is possible.

pseudocode prioritizes clarity and simplicity over strict syntax.

Pseudocode's primary goal is clear communication of logic.

Pseudocode must aim at a broad audience or focusing on procedural steps, I would lean towards `$foo AS Integer` because its explicitness prioritizes immediate understanding over symbolic elegance.

With collaboration with Google Gemini, I (Megacodist) managed to outlined this pseudocode named AlgoDraft with file extension of .algod.

## Benefits
1.	Pseudocode is a way to think out of the box.
2.	It enables everyone to not limit your train of thoughts to any programming language or framework.
3.	It is possible to employ different levels of abstraction. For example, knowing a prefix in string manipulation is a substring starting from the beginning, we can have all of the following:
Leading 🡨 Obtain

# Data types
## Boolean
Variables of this type can hold only `TRUE` and `FALSE` values.

## Integer

## String
Strings are 0-based-indexed sequences of characters.

## Real

## FsPath
Represents a class to instantiate objects to work with file system items.

## File
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

## Function
Any defined procedure and function name is of `Function` type. This is particularly useful for callbacks.

# Basic syntax
## Variables
All variables must start with `$` character and adhere to [camelCase convention](#camelcase).

We use `AS` keyword to specify the type of the variable.

```
$someInt AS Integer
$anotherIntVar AS Integer
```

## Constants
Constants must start with `$` as well and follow [UPPER_SNAKE_CASE convention](#upper_snake_case).

Think of a constant as a named value that never changes while your program (or pseudocode logic) is running. It's like writing down an important number or piece of text on a sticky note and agreeing you'll never scribble it out or change it.

Constants help:

* **Readability**: Using a name like `$MAX_LOGIN_ATTEMPTS` is much clearer than just seeing the number 3 somewhere in your code.

* **Maintainability**: If a value used in many places needs to change (e.g., a tax rate), you only need to change it once where the constant is defined.

* **Preventing Errors**: Clearly marking something as constant helps prevent accidental changes to values that shouldn't be modified.

#### Syntax
```
CONST $CONSTANT_NAME AS DataType <- value
```
Let's break that down:

* `CONST`: This keyword signals that you are defining a constant.

* `$CONSTANT_NAME`: This is the identifier (name) you give your constant.
It must start with `$`. The part after the $ should follow the [SCREAMING_SNAKE_CASE convention](#screaming_snake_case-upper_snake_case-or-macro_case).

* `AS DataType`: Defines the type of the constant.

* `<-`: The assignment operator. You're assigning the value on the right to the name on the left.

* `value`: This is the actual fixed value the constant will hold. It can be a number, text (usually in quotes), a boolean (`TRUE`/`FALSE`), etc.

## Comments
We are using `//` or `ℹ️` to start comments.

## Keywords
This version of pseudocode has lots of keywords and they must be typed in all uppercase. All uppercase is reserved for keywords.

## Natural Language Instructions
Sometimes we need to describe an action rather than imperatively coding it. In such cases we use Natural Language Instructions (NLIs) which consist of all-lower-case text right after `DO:` keyword. 
#### Examples
```
DO: validate the order items for stock availability
DO: add 3 to $idx
```

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

### Indenting Line Continuations
When a single logical instruction or statement is too long to fit comfortably on one line, subsequent lines belonging to that same instruction should be indented. Every time you continue a long line onto the next, you should ident the continuation line or lines by 3 spaces.

```
$longVariableName <- CalculateValue($param1, $param2) +
   AnotherFunctionCall($param3, $param4) - $someOffset

IF ($conditionA AND $conditionB) OR
   ($conditionC AND $conditionD) THEN
   // Instructions inside IF...
ENDIF
```

## Assignments
Assignment is used to assign a new value to a variable. Any left-pointing character or a combination of two that has the bolder appearance is acceptable:
```
$var AS Integer
$var <- 2
$var ◀━ 3
```

## Input from user
The `INPUT` statement is all about receiving an input from the, without any assumption regarding the communication channel (device):
```
$input AS String
$age AS Integer


$input <- INPUT A String  // Waits until the user inputs a string
$input <- INPUT A CHAR    // Waits until the user presses any key
$age <- INPUT AN Integer  // Waits until the user inputs an integer
```

## Output to user
The `OUTPUT` statement is used to show a textual data to the user, without any assumption regarding the communication channel (device):

#### Syntax
In this statement, it is possible to provide a comma-separated sequence of expressions to evaluate before outputing. The value of each expression (string, number, boolean) must be converted to text brfore outputing. Numbers, booleans, objects are typically shown in their standard textual representation.
```
OUTPUT <expression1>, <expression2>, ...
```

#### Characteristics:
* Primarily text-based.
* Generally sequential; one output follows the previous one.
* Does not typically imply graphical elements like dialog boxes or status bars.

#### Example
```
OUTPUT "Starting process..." // Display initial message

DO: do some processing

OUTPUT "\nProcess finished." // Display final summary
```

## User Notifications
This statement actively communicates information, status, or alerts directly to the user through a mechanism designed to get their attention, distinct from a simple text stream.

It usually abstracts away specific methods like:
* Dialog boxes
* Pop-up alerts
* GUI changes like status bars, notification panels etc.
* Sounds

For reporting status or erros during API calls, `RETURN $STATUS_CODE` or `RAISE ErrorType` must be used. `NOTIFY` is all about hiding the complexity of notifying the end user.

#### Characteristics
* Intended to actively inform or alert the user.
* Abstracts the specific UI implementation (could be graphical, audible, etc.).
* Used for important events: task completion, errors needing user attention and/or intervention, confirmations, warnings.
* Distinct from `RETURN` (data back to caller) and `RAISE` (exceptions for caller).

#### Examples
```
$serviceUrl AS String <- "server.example.com"

TRY
   OUTPUT "Attempting connection to " + $serviceAddress + "..."
   $conn <- DO: connecting to the $serviceUrl & returning the connection
      object
CATCH connection failure
   NOTIFY connection failure
ELSE
    // Connection succeeded, letting the user know clearly...
    NOTIFY successfully connection to the endpoint
ENDTRY
```

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
$numbers <- INPUT A List<Integer>

IF ($numbers IS EMPTY) THEN
   NOTIFY the list is empty as error
   DO exit with a suitable return code
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
* Calling functions using positional arguments
* Calling functions using named arguments
* Calling functions using both positional and named arguments (mixed call)

### Calling Functions using Positional Arguments
This is the simplest invocation method, mirroring mathematical notation. Arguments are matched to parameters based strictly on their order. The first argument in the call corresponds to the first parameter in the definition, the second argument to the second parameter, and so on.

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





### Example
```
FUNCTION ProcessData RETURNS Boolean // Returns TRUE on success, FALSE on failure
   PARAMS
      MUTABLE $dataList AS List<Record> // REQUIRED: This list might be modified
      $logFile AS String                // REQUIRED: Path to log file
      $filter AS String <- ""           // OPTIONAL: Filter criteria, defaults to empty
      $strictMode AS Boolean <- FALSE   // OPTIONAL: Flag, defaults to FALSE
   ENDPARAMS

   // ... body uses $dataList, $filter, $strictMode, $logFile ...
   // ... potentially modifies $dataList because it's MUTABLE ...

   RETURN TRUE // Example return
ENDFUNCTION


// Assuming myList exists and is a List<Record>
// Assuming logPath exists and is a String
$success AS Boolean

$success <- ProcessData($myList, $logPath) // $filter defaults "", $strictMode defaults FALSE
$success <- ProcessData($myList, $logFile <- $logPath, $filter <- "active") // OK, $strictMode defaults FALSE
$success <- ProcessData($myList, $logFile <- $logPath, filter: "active", strictMode: TRUE) // All specified
```


## Streams
Streams produce a sequence of values over time, often lazily. They **yield** values one by one, maintaining their internal state between calls.








# Data Structures

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


# Intermediate Syntax
## Error handling
The `TRY-CATCH-ELSE-FINALLY` statement allows you to gracefully handle errors (exceptions) that might occur during the execution of a specific block of code. It prevents your program/algorithm from crashing unexpectedly and lets you define specific recovery actions, logging, or cleanup procedures.

#### Syntax
```
TRY
   // Block 0: Code that might cause an error.
   // This is the code you want to "protect".
   <block_0: potentially problematic instructions>

CATCH <ErrorType1> [AS $errorInfo1] // Optional: Catch a specific error type
   // Block 1: Executes ONLY if ErrorType1 occurred in Block 0.
   <block_1: handling instructions for ErrorType1>

CATCH <ErrorType2> [AS $errorInfo2] // Optional: Catch another specific error
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
* `CATCH <ErrorType> [AS $errorInfo]` (specific): Defines a handler for a specific type of error. If that error occurs in the `TRY` block, the corresponding `CATCH` block executes. The optional `AS $errorInfo` part lets you capture details about the error into a variable (if needed). You can have multiple specific `CATCH` blocks.
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
CATCH file does not exist
   NOTIFY the log path does not exist
CATCH some other file system error
   NOTIFY the probability of issues in the file system
CATCH
   NOTIFY an error occurred
FINALLY
   IF $log IS NULL THEN
      DO: exit the program with a suitable return code
   ENDIF 
ENDTRY
```








# Appendix 1: Naming conventions
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