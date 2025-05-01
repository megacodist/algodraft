---
status: Writing
---
The essence of a string is a **linear, homogeneous, immutable, iterable, and duplicable** data structure in AlgoDraft:

- **Linear**: Strings are organized as a **sequence of characters**, where each character occupies a specific **position** in the chain.
    
    - Each character has a **predecessor** (except the first) and a **successor** (except the last).
    
    - Characters are accessed by **zero-based index** (0, 1, 2, …).
    
    - The structure reflects the **natural reading order** of text.
    
- **Homogeneous**: All elements in a string are of the **same type** — characters. No mixing of types is allowed in the character sequence.

- **Immutable**: Strings **cannot be modified** after creation. Any operation that appears to change a string (e.g., replacing a character) actually creates a **new string**.

- **Iterable**: Strings can be traversed from the first character to the last using constructs like `FOR EACH`, respecting their internal character order.

- **Duplicable**: Characters may **repeat freely** within a string. There is no restriction on frequency or pattern.

They are used to represent **textual data** in a structured and reliable form, where **order matters**, **type uniformity is expected**, and **immutability reinforces safety and clarity** in manipulation.


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
