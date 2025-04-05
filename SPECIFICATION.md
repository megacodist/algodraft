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
The `OUTPUT` statement is all about showing something to the user, without any assumption regarding the communication channel (device):

### Syntax
```
OUTPUT <literal_or_variable>, <literal_or_variable>, ...
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

# Procedures, Functions, and Streams
## Procedures

## Functions

## Streams








# Data Structures

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
      * The system looks for a matching `CATCH` block (first specific, then generic).
      * If a matching `CATCH` is found, its code executes.
      * The `ELSE` block is SKIPPED.
      * The `FINALLY` block executes (if present).
      * Execution continues after `ENDTRY`.
3.	If NO matching `CATCH` is found, the error might propagate outwards (or crash the program, depending on higher-level handling). The `FINALLY` block still runs.








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