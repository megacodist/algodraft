---
status: Draft
---
AlgoDraft offers the flexibility to combine positional and named arguments in a single function call. The caller must provide some initial arguments positionally, and then switch to using named arguments for the remaining ones.

**Positional-before-named rule**:
All positional arguments **MUST** come **BEFORE** all named arguments. You cannot place a positional argument after a named argument in the same call.

**Syntax**:
```
DoSomething(posArg1, posArg2, ..., $namedArg1 <- value1, $namedArg2 <- value2, ...)
```

**Pros:**
- Offers a balance – concise positional arguments for common, leading parameters, and clear named arguments for less frequent, optional, or later parameters.  

**Cons:** 
- Requires adherence to the positional-before-named rule.

**Example 1: Using with optional parameters**

```
FUNCTION CreateButton(
	    IN $text AS String,
	    IN $width AS Integer <- 100,
	    IN $height AS Integer <- 30,
	    IN $color AS String <- "Blue"
	) :=
	DO {{create a GUI button showing $text text in rectangle of $width X $height with background color of $color}}
ENDFUNCTION

CreateButton("Help", $color <- "Green")
// $text is "Help" (positional)
// $width uses default 100
// $height uses default 30
// $color is "Green" (named)
// GUI button using Text: 'Help', Size: 100x30, Color: Green

CreateButton("OK", 80, $color <- "Gray")
// $text is "OK" (positional)
// $width is 80 (positional)
// $height uses default 30
// $color is "Gray" (named)
// GUI button using Text: 'OK', Size: 80x30, Color: Gray
```

**Example: Invalid mixed call (for illustration):**

```
// This would be an ERROR in AlgoDraft:
// CreateButton($text: "Error", 70) // Positional argument '70' after named argument '$text'
```
