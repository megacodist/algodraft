---
status: Draft
---
At their core, computers execute sequences of instructions. However, much of the power of programming comes from the ability to make decisions and control the flow of execution based on certain conditions. Is a value greater than 10? Has an error occurred? Is a user's input valid? Answering these kinds of yes/no questions is fundamental. This decision-making process is deeply rooted in the principles of logic, which provides a formal way to reason about truth and falsehood.

While logic can encompass complex systems, the most fundamental and widely used form in computer science is **Boolean logic**, named after mathematician George Boole. Boolean logic deals with exactly two possible values: **`TRUE`** and **`FALSE`**. This two-state system maps perfectly onto the binary nature of digital electronics, where circuits are typically in one of two states (like 'on' or 'off', or 'high voltage' or 'low voltage'). Because of this fundamental alignment, virtually all processing and decision-making within a computer ultimately relies on Boolean logic. Therefore, within AlgoDraft and much of programming, when we refer to 'logic' or 'logical' values/operations/expressions, we specifically mean **Boolean** logic and the values **`TRUE`** and **`FALSE`**.

# Boolean/Logical Literals and Variables

In AlgoDraft, the two fundamental Boolean values are represented by the keywords:

- **`TRUE`**: Represents the concept of truth, affirmation, or 'yes'.

- **`FALSE`**: Represents the concept of falsehood, negation, or 'no'.

These are the only two literal values that a Boolean type can directly hold. As per AlgoDraft's casing conventions, these keywords **must** be written in **uppercase**.

To store a Boolean value (either `TRUE` or `FALSE`) or the result of a logical expression within your algorithm, you use a variable declared with the Boolean data type.

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

# Boolean/Logical Operators

Just as we have basic arithmetic operators like +, -, \*, and / to work with numbers, there is a fundamental set of **Boolean operators** (also called **logical operators**) specifically designed to operate on Boolean values (`TRUE` and `FALSE`).

These operators aren't just arbitrary rules; they are particularly important because they serve as precise **mathematical models** for fundamental aspects of **human logical thinking and reasoning**. They capture core ways we connect or modify conditions. By formalizing these logical connections into unambiguous mathematical operations, Boolean operators allow us to build reliable and predictable decision-making logic within our algorithms.

| Operation   | Keywords | Symbols   | Meaning                                                    |
| ----------- | -------- | --------- | ---------------------------------------------------------- |
| Logical NOT | `NOT`    | `¬`       | Inverts the condition.                                     |
| Logical AND | `AND`    | `/\`, `∧` | `TRUE` if both operands are `TRUE` otherwise `FALSE`.      |
| Logical XOR | XOR      | `⊻`       | `TRUE` if exactly one operand is `TRUE` otherwise `FALSE`. |
| Logical OR  | `OR`     | `\/`, `∨` | `FALSE` if both operands are `FALSE` otherwise `TRUE`.     |

**Notes**
* Use symbolic forms when writing algorithms that emphasize mathematical clarity, and use keyword forms when focusing on readability.
* These operators are listed according to their relative priority.

## Logical Negation

The `NOT` operator is the simplest. It is a **unary** operator, meaning it acts on a single Boolean value or expression. It simply **reverses** the truth value of its operand. If the operand is `TRUE`, `NOT` makes it `FALSE`. If the operand is `FALSE`, `NOT` makes it `TRUE`. This models the concept of negation ("It is **not** the case that...").

**Truth Table:**

| A       | NOT A<br>¬A |
| ------- | ----------- |
| `TRUE`  | `FALSE`     |
| `FALSE` | `TRUE`      |

**Example:**

```
$isActive AS Boolean <- TRUE
$isDisabled AS Boolean
$isDisabled <- NOT $isActive // $isDisabled becomes FALSE

$isEmpty AS Boolean <- FALSE
$isNotEmpty AS Boolean
$isNotEmpty <- NOT $isEmpty // $isNotEmpty becomes TRUE
```

## Logical AND

The `AND` operator is a **binary** operator, meaning it combines two Boolean values or expressions (operands). The `AND` operation results in `TRUE` **if and only if both of its operands are `TRUE`**. If either operand (or both) is `FALSE`, the result is `FALSE`. This models the requirement for multiple conditions to be met simultaneously ("Condition A **and** Condition B must both be true").

**Truth Table:**

| A       | B       | A AND B<br>A /\ B<br>A ∧ B |
| ------- | ------- | -------------------------- |
| `TRUE`  | `TRUE`  | `TRUE`                     |
| `TRUE`  | `FALSE` | `FALSE`                    |
| `FALSE` | `TRUE`  | `FALSE`                    |
| `FALSE` | `FALSE` | `FALSE`                    |

**Short circuit:**

In `AND`, if the left operand is `FALSE`, the right operand will not be evaluated.

**Example:**

```
$isLoggedIn AS Boolean <- TRUE
$hasAdminRights AS Boolean <- FALSE
$canAccessSystem AS Boolean
$canAccessSystem <- $isLoggedIn AND $hasAdminRights // $canAccessSystem becomes FALSE

$isValidInput AS Boolean <- TRUE
$isWithinRange AS Boolean <- TRUE
$isDataAcceptable AS Boolean
$isDataAcceptable <- $isValidInput AND $isWithinRange // $isDataAcceptable becomes TRUE
```

## Logical OR

The `OR` operator is also a **binary** operator. The `OR` operation results in `TRUE` **if at least one of its operands is `TRUE`**. It only results in `FALSE` if both operands are `FALSE`. This models the situation where satisfying any one of several conditions is sufficient ("Condition A **or** Condition B (or both) must be true"). This is sometimes called an "inclusive OR".

**Truth Table:**

| A       | B       | A OR B<br>A \\/ B<br>A ∨ B |
| ------- | ------- | -------------------------- |
| `TRUE`  | `TRUE`  | `TRUE`                     |
| `TRUE`  | `FALSE` | `TRUE`                     |
| `FALSE` | `TRUE`  | `TRUE`                     |
| `FALSE` | `FALSE` | `FALSE`                    |
**Short-circuit**:

In `OR`, if the left operand is `TRUE`, the right operand will not be evaluated.

**Example:**

```
$hasCoupon AS Boolean <- FALSE
$isMember AS Boolean <- TRUE
$getsDiscount AS Boolean
$getsDiscount <- $hasCoupon OR $isMember // $getsDiscount becomes TRUE

$isEmergency AS Boolean <- FALSE
$isPriority AS Boolean <- FALSE
$needsImmediateAction AS Boolean
$needsImmediateAction <- $isEmergency OR $isPriority // $needsImmediateAction becomes FALSE
```

## Logical XOR

The `XOR` operator (Exclusive OR) is a **binary** operator that results in `TRUE` **if and only if its operands have different truth values**. If both operands are `TRUE` or both are `FALSE`, the result is `FALSE`. This models the situation where exactly one of two conditions must be true, but not both ("Either Condition A **or** Condition B is true, **but not both**").

**Truth Table:**

| A     | B     | A XOR B<br>A ⊻ B |
| ----- | ----- | ---------------- |
| TRUE  | TRUE  | FALSE            |
| TRUE  | FALSE | TRUE             |
| FALSE | TRUE  | TRUE             |
| FALSE | FALSE | FALSE            |

**Example:**

```
// Example: A light switch controlled by two switches (common XOR example)
$switch1_On AS Boolean <- TRUE
$switch2_On AS Boolean <- FALSE
$lightIsOn AS Boolean
$lightIsOn <- $switch1_On XOR $switch2_On // $lightIsOn becomes TRUE (one switch is on, one is off)

// If both switches were on:
$switch1_On <- TRUE
$switch2_On <- TRUE
$lightIsOn <- $switch1_On XOR $switch2_On // $lightIsOn becomes FALSE (both are the same)

// Example: Check if exactly one condition is met
$optionA_Selected AS Boolean <- FALSE
$optionB_Selected AS Boolean <- TRUE
$exactlyOneOptionChosen AS Boolean
$exactlyOneOptionChosen <- $optionA_Selected XOR $optionB_Selected // $exactlyOneOptionChosen becomes TRUE
```

## Operator Precedence and Parentheses

When combining multiple logical operators in a single expression, AlgoDraft follows a specific order of operations (precedence):

1. **NOT** has the highest precedence.
    
2. **AND** has the next highest precedence.
    
3. **XOR** has the next highest precedence.
    
4. **OR** has the lowest precedence.
    

Note: Define the exact precedence clearly in AlgoDraft's documentation. This order (NOT > AND > XOR > OR) is common but verify/specify for AlgoDraft.

This means NOT operations are evaluated first, then AND, then XOR, then OR.