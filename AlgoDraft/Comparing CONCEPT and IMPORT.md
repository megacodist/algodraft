---
status: Writing
---
The following table compare these two statements:

| Feature                 | `CONCEPT ... ENDCONCEPT`                                                                                                                           | `IMPORT ... ENDIMPORT`                                                                                                                                                                                             |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Primary Role**        | **Design Tool:** Declares abstract, conceptual external entities.                                                                                  | **Implementation Bridge:** Makes specific, known external entities accessible.                                                                                                                                     |
| **Knowledge State**     | Assumes an idea or idealized interface exists. Details are via Natural Language Descriptions (NLDs) using `:= {{NLD}}`.                            | Assumes a concrete entity exists in a specified external source (`FROM external.source`).                                                                                                                          |
| **Focus**               | Algorithm logic, OS/library agnosticism, high-level dependencies defined by NLDs.                                                                  | Interfacing with specific external components, managing dependencies on named sources.                                                                                                                             |
| **Function Signatures** | **Mandatory** full AlgoDraft signature required, then `:= {{NLD}}`.                                                                                | **Optional:** Can import by name only (signature is external) OR with a full AlgoDraft signature.                                                                                                                  |
| **Source Specificity**  | NLD describes origin conceptually.                                                                                                                 | `FROM external.source` explicitly names the origin.                                                                                                                                                                |
| **Use Case**            | - Early-stage, high-level design. <br> - Platform-neutral algorithm specification. <br> - Defining idealized interactions with the external world. | - More detailed design closer to implementation. <br> - Illustrating use of specific (even if still conceptual) libraries/modules. <br> - Defining clearer contracts for external function calls within AlgoDraft. |

**When to Use Which:**

- Use **`CONCEPT`** when your primary goal is to design the algorithm's logic with maximal abstraction from specific external implementations. You define what you need conceptually, and the NLD explains it.

- Use **`IMPORT`** when you are ready to acknowledge more concrete external sources for your dependencies. You specify where an entity comes from. For functions, you have the choice to either trust the external signature completely (import by name) or to provide an AlgoDraft-compatible signature for enhanced clarity within your design.

**Example: combining `CONCEPT` and `IMPORT`**

Once types, constants, and functions are declared via `CONCEPT` or imported via `IMPORT`, they can be used in your AlgoDraft algorithms just like entities defined directly within AlgoDraft.

```
CONCEPT
    TYPE UserPreferences := {{A structure holding user-specific application preferences.}}
    FUNCTION LoadPreferences(IN $userID AS String) -> UserPreferences
        := {{Loads user preferences from a conceptual persistent store for the given user ID.}}
ENDCONCEPT

IMPORT
    TYPE FileHandle FROM OS.IO
    FUNCTION LogMessage(IN $message AS String, IN $level AS Integer) FROM System.Logger AS Log
    FUNCTION ReadTextFromFile(IN $handle AS FileHandle) -> String FROM OS.IO.File
    FUNCTION OpenFileForReading(IN $filePath AS String) -> FileHandle FROM OS.IO.File
ENDIMPORT

FUNCTION InitializeUserSettings(
		IN $currentUserID AS String,
		IN $logFilePath AS String,
		) :=
    $prefs AS UserPreferences
    $logFileHandle AS FileHandle
    $initialLogData AS String

    // Using a CONCEPTual function
    $prefs <- LoadPreferences($currentUserID)
    Log("Preferences loaded for user: " + $currentUserID, 0) // Using imported & aliased Log

    // Using IMPORTed functions and types
    $logFileHandle <- OpenFileForReading($logFilePath)
    IF {{$logFileHandle is valid}} THEN // Assuming some way to check FileHandle validity
        $initialLogData <- ReadTextFromFile($logFileHandle)
        // ... process $initialLogData and $prefs ...
        Log("Initial log data processed.", 1)
    ELSE
        Log("Failed to open log file: " + $logFilePath, 2) // Log level 2 for error
    ENDIF
    // ... further logic using $prefs ...
ENDFUNCTION
```

This example demonstrates how both `CONCEPT` (for high-level abstract needs like `UserPreferences`) and `IMPORT` (for more "concrete" external utilities like logging and file I/O) can coexist to build a comprehensive algorithm design. The choice of importing functions by name or with a signature allows the designer to control the level of detail and assumed knowledge within the AlgoDraft specification.

