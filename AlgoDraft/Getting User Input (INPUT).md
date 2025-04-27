---
status: Draft
---
When your algorithm needs data from the user during execution, use the `INPUT` keyword. This keyword:
* involves external interaction.
* waits until the user provides the requested input.
* is followed by a simple natural language description of what you expect the user to provide.
* The value entered by the user is returned by the statement and usually assigned to a variable.

**Syntax**
```
$variable AS DataType <- INPUT <nld_data_description>
```

In this syntax, `<nld_data_description>` is the [[Natural Language Descriptions|Natural Language Description]] following `INPUT`:
* **Role**: Its primary role is to describe the specific piece of data the user is expected to provide.
* **Form**: Typically a noun phrase or descriptive sentence fragment identifying the required input (e.g., "the user's age", "a valid email address", "the path to the input file", "'Y' to confirm, 'N' to cancel"). It is not an imperative command like with `DO`.

**Example**

```
$userName AS String
$userAge AS Integer
$nums AS List<Integer>


$userName <- INPUT {{the user's full name}}    // Waits until the user inputs a string
$userAge <- INPUT {{the user's age as an integer}}  // Waits until the user inputs an integer
$nums <- INPUT {{get some Integers from user}} // Waits until the user inputs a list of integers
$keyPressed AS String <- INPUT {{any keyboard stroke}}  // Waits until the user presses any key
```
