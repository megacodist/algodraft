---
status: Draft
---
In our pseudocode dialect, we need a way to represent the algorithm communicating information outwards to the user. While traditional pseudocode often uses `PRINT`,  `OUTPUT`, or `DISPLAY` (usually implying console output), our dialect uses the more abstract `NOTIFY` keyword.

The `NOTIFY` statement signifies that the algorithm should make the user aware of something. It abstracts away the specific method of notification (e.g., printing to a terminal, showing a dialog box, updating a status bar, playing a sound, etc.). It focuses purely on the intent to inform.

**Characteristics**
* **User-Facing**: It's intended for information directed at the human user.
* **Non-Interactive**: It's a one-way communication. The algorithm presents the information and continues execution immediately; it does not wait for user acknowledgement or input. (If interaction is needed, use an [[Getting User Input (INPUT)|INPUT statement]] afterwards).
* **Abstract**: It describes what the user should be notified about, not how.
* **Severity Indication**: The description should include an indication of the notification's urgency or type (e.g., info, warning, error).

**Syntax**
```
NOTIFY {{<nld_event_description> [as <severity_level>}}
```

* **`NOTIFY`**: The keyword indicating a user notification action.

* **`<nld_event_description>`**: The Natural Language Description following `NOTIFY`.
   * **Role**: Its primary role is to describe the event, status, or piece of information the user should be made aware of. It explains what happened or what the current state is.
   * **Form**: A descriptive phrase or sentence. Can and often should include variable values to provide context.

* **`[as <severity_level>]`**: (Highly Recommended) Specifies the optional nature or urgency of the notification. Common levels include:
   * **info**: General informational message.
   * **success**: Confirmation of a successful operation.
   * **warning**: Indicates a potential issue or something the user should be cautious about.
   * **error**: Reports that an operation failed or an error occurred.
   * **critical**: Reports a severe error that might have significant consequences or require immediate attention.

When the pseudocode encounters a `NOTIFY` statement, it means "At this point, the running program should present the described information to the user, classified by the given severity." The person implementing the pseudocode would then choose the most appropriate mechanism based on the description, the severity level, and the application's context (e.g., a GUI application might use a pop-up for critical, while a command-line tool might print in red text).

**Examples**
```
NOTIFY {{the file processing is starting as info}}
// ... processing steps ...
NOTIFY {{file processing completed successfully as success}}

// Implementation Hint: Could be simple console messages or status bar updates. Success notifications could be silent or a brief status message.
```

```
$calculationResult AS Real <- DO {{perform complex calculation}}
NOTIFY {{the final result is $calculationResult as info}}

// Implementation Hint: Could print to console, update a text field in a GUI.

```

```
$itemsLeft AS Integer <- DO {{get items remaining in stock}}
IF $itemsLeft < 10 THEN
    NOTIFY {{inventory level is low ($itemsLeft items remaining) as warning}}
ENDIF

// Implementation Hint: Might be a console message, a non-blocking notification pop-up, or changing an indicator icon's color.
```

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

// Implementation Hint: critical strongly suggests a very visible notification, perhaps a modal dialog
// (even though NOTIFY itself isn't blocking, the response to a critical notification might halt further
// progress), logging, or loud alerts.
```

# Contrast with Other Keywords
* **`NOTIFY` vs. `INPUT`**: `NOTIFY` sends information out non-interactively. `INPUT` pauses and waits to receive information in.
* **`NOTIFY` vs. `DO`**: `NOTIFY` is purely for communicating with the user. `DO` is for describing internal operations or actions the algorithm performs (which might result in a value, assigned using `<- DO`, or just perform an action). Writing to a log file intended only for developers/debugging would likely use `DO {{write ... to log}}`, whereas `NOTIFY` is for the end-user.

# Best Practices:
* **Always Include Severity**: While optional in the syntax, always try to include `as <severity_level>`. It dramatically improves clarity and guides implementation.
* **Be Descriptive**: The natural language part should clearly convey the necessary information. Include relevant variable values.
* **Stay Abstract**: Avoid specifying how to notify (e.g., don't write `NOTIFY {{show a blinking red dialog box...}}`). Let the severity and description guide the implementation.
* **Define Standard Levels (Optional)**: Consider establishing a standard set of severity levels for consistency within your project or team.
