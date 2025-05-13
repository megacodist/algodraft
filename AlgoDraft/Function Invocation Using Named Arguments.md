---
status: Draft
---
Named arguments allow you to explicitly associate an argument with a specific parameter by using the parameter's name in the function call. Arguments are specified using the parameter's name (including the $ sigil), followed by the assignment operator (`<-`), and then the argument's value or variable.

**Pros:**

- Highly readable and self-documenting.

- Order independence prevents errors.

- Eliminates ambiguity about which argument maps to which parameter.

- Easy to specify only certain optional arguments regardless of their position.

**Cons:**

- More verbose than positional arguments.

**Syntax:**

```
FunctionName($paramName1 <- argument1, $paramName2<- argument2, ...)
```

**Example 1: Basic named arguments**

```
FUNCTION DescribePerson($name AS String, $age AS Integer) :=
	$msg AS String <- $name + " is " + $age + " years old."
	NOTIFY {{$msg}}
ENDFUNCTION

DescribePerson($name <- "David", $age <- 25)
// The user is notified "David is 25 years old."
DescribePerson($age <- 40, $name <- "Eve") // Order doesn't matter
// The user is notified "Eve is 40 years old."
```

**Example 2: Clarity with `OUT` and `INOUT` parameters**

```
FUNCTION GetUserDetails (
	    $userId AS Integer,          // Input: ID of the user
	    OUT $userName AS String,        // Output: The user's name
	    OUT $userAge AS Integer         // Output: The user's age
	) :=
    // Simulate fetching data
    IF $userId = 1 THEN
        $userName <- "Alice"
        $userAge <- 30
    ELSE
        $userName <- "Unknown"
        $userAge <- -1
    ENDIF
ENDFUNCTION

$idNum AS Integer <- 2
$fetchedName AS String
$fetchedAge AS Integer

GetUserDetails($userId <- $idNum, $userName <- $fetchedName, $userAge <- $fetchedAge)
// $fetchedName and $fetchedAge will be populated by the function.
```

**Example 3: Flexibility with default-valued parameters**

Named arguments make it very easy to provide arguments for only specific optional parameters, regardless of their position.

```
FUNCTION CreateButton(
	    IN $text AS String,
	    IN $width AS Integer <- 100,
	    IN $height AS Integer <- 30,
	    IN $color AS String <- "Blue"
	) :=
	DO {{create a GUI button showing $text text in rectangle of $width X $height with background color of $color}}
ENDFUNCTION

CreateButton($text: "Submit")
// GUI button using Text: 'Submit', Size: 100x30, Color: Blue

CreateButton($text: "Cancel", $color: "Red")
// GUI button using Text: 'Cancel', Size: 100x30, Color: Red

CreateButton($text: "Reset", $height: 40, $width: 120)
// GUI button using Text: 'Reset', Size: 120x40, Color: Blue
```
