---
status: Draft
---
The `TRY-CATCH-ELSE-FINALLY` statement allows you to gracefully handle errors (exceptions) that might occur during the execution of a specific block of code. It prevents your program/algorithm from crashing unexpectedly and lets you define specific recovery actions, logging, or cleanup procedures.

**Syntax**:

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

**Keywords and Clauses:**

* `TRY`: Identifies the block of code under observation for errors.

* `CATCH <ErrorType> [AS $errorInfo]`: Defines a handler for a specific type of error. If that error occurs in the TRY block, the corresponding CATCH block executes. The optional AS $errorInfo part lets you capture details about the error into a variable (if needed). You can have multiple specific CATCH blocks.

* `CATCH (Generic)`: Catches any error not handled by preceding specific CATCH blocks.

* `ELSE`: Defines a block that runs only if the `TRY` block completes successfully without raising any errors.

* `FINALLY`: Defines a block that always runs after the `TRY` block and any executed `CATCH` or `ELSE` block. It runs whether an error occurred or not, whether it was caught or not. Essential for cleanup.

* `ENDTRY`: Closes the structure.

#### Flow of Execution

1.       Code in the TRY block executes in its usual flow of execution.

2.       If an error occurs:

·         The TRY block stops immediately at the point of error.

·         The system looks for a matching CATCH block (first specific, then generic).

·         If a matching CATCH is found, its code executes.

·         The ELSE block is SKIPPED.

·         The FINALLY block executes (if present).

·         Execution continues after END TRY.

3.       If NO matching CATCH is found, the error might propagate outwards (or crash the program, depending on higher-level handling). The FINALLY block still runs.

4.       If NO error occurs:

·         The entire TRY block completes successfully.

·         All CATCH blocks are SKIPPED.

·         The ELSE block executes (if present).

·         The FINALLY block executes (if present).

·         Execution continues after END TRY.