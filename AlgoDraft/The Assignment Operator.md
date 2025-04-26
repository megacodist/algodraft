---
status: Draft
---
The assignment operator `<-` is used to place a value into a declared variable.

**Syntax**
```
$variableName <- <value>
```

* **`<-`**: The assignment operator. It takes the value on the right and stores it in the variable on the left. You can read it as:
	* The variable named `variableName` **gets** the value `<value>`.
	* The variable named `variableName` **is assigned** the value `<value>`.

* **`<value>`**: The data being assigned. This can be a literal value (like `10`, `"Hello"`, `TRUE`), another variable, or the result of an operation. The value must be compatible with the variable's declared data type.

**Example**
```
$counter AS Integer // Declaration
$counter <- 0       // Assignment
$counter2 AS Integer <- $counter
$counter3 AS Integer <- $counter + 10

$customerName AS String
$customerName <- "Megacodist"
$message AS String <- "Welcome!"

$isActive AS Boolean
$isActive <- TRUE
$isComplete AS Boolean <- FALSE
```

The key characteristic of variables is that their value can be updated using the assignment operator again.

```
$score AS Integer <- 0
NOTIFY {{initial score is $score as info}}

$score <- $score + 10 // Update the value
NOTIFY {{score after update is $score as info}}
```

The `<-` operator is central to both variables and constants:

* **For Variables**: Used both to set the initial value and to update the value later.

* **For Constants**: Used only to set the initial (and permanent) value during definition with the CONST keyword.

It always means "take the value from the right side and store it into the named container on the left side."

| Feature | Variable ($varName) | Constant (CONST $CONST_NAME) |
| ------- | ------------------- | ----------------------------- |
|Purpose | Store changing data | Store fixed data |
| Keyword | None (just `$`) | `CONST` |
| Naming | `$camelCase` | `$UPPER_SNAKE_CASE` |
| Type |	`AS DataType` (Required) | `AS DataType` (Required) |
| Assignment | `<-` (For initialization & updates) | `<-` (Required only at definition) |
| Mutability | Mutable (Can change) | Immutable (Cannot change after definition) |
