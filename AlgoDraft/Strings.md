---
status: Writing
---
# Substrings
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
