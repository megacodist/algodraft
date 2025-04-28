---
status: Draft
---
In the Megacodist dialect of pseudocode, algorithms need a way to communicate information outwards to the user. While traditional pseudocode dialects often uses `PRINT`,  `OUTPUT`, or `DISPLAY` (usually implying console output), AlgoDraft uses the more abstract `NOTIFY` keyword.

The `NOTIFY` statement signifies that the algorithm should make the user aware of something. It abstracts away the specific method of notification (e.g., printing to a terminal, showing a dialog box, updating a status bar or other part of the GUI, playing a sound, etc.). It focuses purely on the intent to inform.

**Characteristics**
* **User-Facing**: It's intended for information directed at the human user.
* **Non-Interactive**: It's a one-way communication. The algorithm presents the information and continues execution immediately; it does not wait for user acknowledgement or input. (If interaction is needed, use an [[Getting User Input (INPUT)|INPUT statement]] afterwards).
* **Abstract**: It describes what the user should be notified about, not how.
* **Severity Indication**: The description should include an indication of the notification's urgency or type (e.g., info, warning, error).

**Syntax**
```
NOTIFY {{<nld_event_or_fact> [as <severity_level>}}
```

* **`NOTIFY`**: The keyword indicating a user notification action.

* **`<nld_event_or_truth>`**: The Natural Language Description following `NOTIFY`.
   * **Role**: Its primary role is to describe the event, status, or piece of information the user should be made aware of. It explains what happened or what the current state is.
   * **Form**: A descriptive phrase or sentence, usually of an event or a fact inside the program. Can and often should include variable values to provide context.

* **`[as <severity_level>]`**: (Highly Recommended) Specifies the optional nature or urgency of the notification. Common levels include:
   * **info**: General informational message.
   * **success**: Confirmation of a successful operation.
   * **warning**: Indicates a potential issue or something the user should be cautious about.
   * **error**: Reports that an operation failed or an error occurred.
   * **critical**: Reports a severe error that might have significant consequences or require immediate attention.

When the pseudocode encounters a `NOTIFY` statement, it means "At this point, the running program should present the described information to the user, classified by the given severity." The person implementing the pseudocode would then choose the most appropriate mechanism based on the description, the severity level, and the application's context (e.g., a GUI application might use a pop-up for critical, while a command-line tool might print in red text).

**Example 1**:

```
NOTIFY {{the file processing is started as info}}
// ... processing steps ...
NOTIFY {{file processing completed successfully as success}}
```

**Implementation Hint**: Could be simple console messages or status bar updates. Success notifications could be silent or a brief status message.

**Example 2**:

```
// Finding min & max...
$data AS List<Integer> <- [4, 1, 8, 3]
$minValue AS Integer     // Variable to receive minimum
$maxValue AS Integer     // Variable to receive maximum
FindMinMax($data, $minResult <- $minValue, $maxResult <- $maxValue)
NOTIFY {{minimum and maximum are $minValue and $maxValue respectively as info}}
```

**Implementation Hint**: Could print to console, update a text field in a GUI.

**Example 3**:

```
$itemsLeft AS Integer <- DO {{get items remaining in stock}}
IF $itemsLeft < 10 THEN
    NOTIFY {{inventory level is low ($itemsLeft items remaining) as warning}}
ENDIF
```

**Implementation Hint**: Might be a console message, a non-blocking notification pop-up, or changing an indicator icon's color.

**Example 4**:
```
$fileHandle AS File <- DO {{attempt to open important_data.log for writing}}

IF $fileHandle IS NULL THEN // Assuming NULL indicates failure
    NOTIFY {{failed to open important_data.log for writing as critical}}
    // Algorithm might terminate or take recovery steps here
ELSE
    NOTIFY {{successfully opened important_data.log as success}}
    // ... proceed to write ...
    DO {{close $fileHandle}}
ENDIF
```

**Implementation Hint**: critical strongly suggests a very visible notification, perhaps a modal dialog (even though NOTIFY itself isn't blocking, the response to a critical notification might halt further progress), logging, or loud alerts.

# Contrast with Other Keywords
* **`NOTIFY` vs. `INPUT`**: `NOTIFY` sends information out non-interactively. `INPUT` pauses and waits to receive information in.
* **`NOTIFY` vs. `DO`**: `NOTIFY` is purely for communicating with the user. `DO` is for describing internal operations or actions the algorithm performs (which might result in a value, assigned using `<- DO`, or just perform an action). Writing to a log file intended only for developers/debugging would likely use `DO {{write ... to log}}`, whereas `NOTIFY` is for the end-user.

# Best Practices:
* **Always Include Severity**: While optional in the syntax, always try to include `as <severity_level>`. It dramatically improves clarity and guides implementation.
* **Be Descriptive**: The natural language part should clearly convey the necessary information. Include relevant variable values.
* **Stay Abstract**: Avoid specifying how to notify (e.g., don't write `NOTIFY {{show a blinking red dialog box...}}`). Let the severity and description guide the implementation.
* **Define Standard Levels (Optional)**: Consider establishing a standard set of severity levels for consistency within your project or team.
