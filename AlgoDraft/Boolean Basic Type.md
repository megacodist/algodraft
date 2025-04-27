---
status: Draft
---
In AlgoDraft, the **Boolean** type represents one of two possible logical states: **`TRUE`** or **`FALSE`**.  
Booleans are fundamental for controlling the flow of an algorithm, such as making decisions, repeating actions, and evaluating conditions.

# Boolean Literals and Variables

AlgoDraft provides two built-in literals for the Boolean type:

|Literal|Meaning|
|---|---|
|`TRUE`|Represents logical truth.|
|`FALSE`|Represents logical falsehood.|

- As being keywords, Boolean literals must be written **in uppercase**: `TRUE`, `FALSE`.
    
- These values can be assigned directly to variables or used within expressions.

**Example**

```
$operationFlag AS Boolean <- TRUE
$pauseAvailable <- FALSE

NOTIFY {{the condition of the operation is $operationFlag}}    // TRUE
NOTIFY {{pause availability is $pauseAvailable}}  // FALSE
```

Here:
- `$operationFlag` is a Boolean variable initialized with `TRUE`.
    
- `$pauseAvailable` is a Boolean variable initialized with `FALSE`.
    
- `NOTIFY` informs the value of each variable.

# Logical Operators

In AlgoDraft, **logical operators** allow you to combine or modify Boolean values to create more complex conditions. They are essential for decision-making and control flow in algorithms. AlgoDraft provides both **keyword** and **symbolic** forms for logical operations.

| Operation   | Keywords | Symbols   | Meaning                                                |
| ----------- | -------- | --------- | ------------------------------------------------------ |
| Logical AND | `AND`    | `/\`, `∧` | `TRUE` if both operands are `TRUE` otherwise `FALSE`.  |
| Logical OR  | `OR`     | `\/`, `∨` | `FALSE` if both operands are `FALSE` otherwise `TRUE`. |
| Logical NOT | `NOT`    | `¬`       | Inverts the condition.                                 |

**Notes**
* Use symbolic forms (`∧`, `∨`, `¬`) when writing algorithms that emphasize mathematical clarity, and use keyword forms (`AND`, `OR`, `NOT`) when focusing on readability.
* **Operator Precedence**: `NOT` has higher precedence than `AND`, which has higher precedence than `OR`.
- **Short-circuit Evaluation** (if implemented):
    - In `AND`, if the left operand is `FALSE`, the right operand might not be evaluated.
    - In `OR`, if the left operand is `TRUE`, the right operand might not be evaluated.

## **Logical AND**

The **`AND`** operation returns `TRUE` only if **both** operands are `TRUE`.

|Left Operand|Right Operand|Result (`AND`)|
|---|---|---|
|TRUE|TRUE|TRUE|
|TRUE|FALSE|FALSE|
|FALSE|TRUE|FALSE|
|FALSE|FALSE|FALSE|
## **Logical OR**

The **`OR`** operation returns `TRUE` if **at least one** operand is `TRUE`.

| Left Operand | Right Operand | Result (`OR`) |
| ------------ | ------------- | ------------- |
| TRUE         | TRUE          | TRUE          |
| TRUE         | FALSE         | TRUE          |
| FALSE        | TRUE          | TRUE          |
| FALSE        | FALSE         | FALSE         |
## **Logical NOT**

The **`NOT`** operation **inverts** the value of a single Boolean.

|Operand|Result (`NOT`)|
|---|---|
|TRUE|FALSE|
|FALSE|TRUE|

## Example
```
$a <- TRUE
$b <- FALSE

$result1 <- $a AND $b       // FALSE
$result2 <- $a ∧ $a         // TRUE
$result3 <- $a OR $b        // TRUE
$result4 <- $b \/ $b        // FALSE
$result5 <- NOT $a          // FALSE
$result6 <- ¬$b             // TRUE
```

