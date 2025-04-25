---
status: Draft
---
The assignment operator `<-` is used to place a value into a declared variable.

#### Syntax
```
$variableName <- <value>
```

* **`<-`**: The assignment operator. Read it as "gets" or "is assigned". It takes the value on the right and stores it in the variable on the left.

* **`<value>`**: The data being assigned. This can be a literal value (like `10`, `"Hello"`, `TRUE`), another variable, or the result of an operation. The value must be compatible with the variable's declared data type.

#### Example
```
$counter AS Integer // Declaration
$counter <- 0       // Assignment

$customerName AS String
$customerName <- "Bob"

$isActive AS Boolean
$isActive <- TRUE
```

#### Declaration with Initialization Syntax
You can declare a variable and assign its initial value in a single step.
```
$variableName AS DataType <- <initialValue>
```

#### Example
```
$score AS Integer <- 0
$message AS String <- "Welcome!"
$isComplete AS Boolean <- FALSE
```

#### Changing Variable Values

The key characteristic of variables is that their value can be updated using the assignment operator again.

```
$score AS Integer <- 0
NOTIFY: Initial score is $score as info // Notifies: Initial score is 0...

$score <- $score + 10 // Update the value
NOTIFY: Score after update is $score as info // Notifies: Score after update is 10...
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
