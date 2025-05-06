---
status: Draft
---
At their core, computers execute sequences of instructions. However, much of the power of programming comes from the ability to make decisions and control the flow of execution based on certain conditions. Is a value greater than 10? Has an error occurred? Is a user's input valid? Answering these kinds of yes/no questions is fundamental. This decision-making process is deeply rooted in the principles of logic, which provides a formal way to reason about truth and falsehood.

While logic can encompass complex systems, the most fundamental and widely used form in computer science is **Boolean logic**, named after mathematician George Boole. Boolean logic deals with exactly two possible values: **`TRUE`** and **`FALSE`**. This two-state system maps perfectly onto the binary nature of digital electronics, where circuits are typically in one of two states (like 'on' or 'off', or 'high voltage' or 'low voltage'). Because of this fundamental alignment, virtually all processing and decision-making within a computer ultimately relies on Boolean logic. Therefore, within AlgoDraft and much of programming, when we refer to 'logic' or 'logical' values/operations/expressions, we specifically mean **Boolean** logic and the values **`TRUE`** and **`FALSE`**.

# Boolean/Logical Literals

In AlgoDraft, the two fundamental Boolean values are represented by the keywords:

- **`TRUE`**: Represents the concept of truth, affirmation, or 'yes'.

- **`FALSE`**: Represents the concept of falsehood, negation, or 'no'.

These are the only two literal values that a Boolean type can directly hold. As per AlgoDraft's casing conventions, these keywords **must** be written in **uppercase**.

# Boolean/Logical Variables

To store a Boolean value (either `TRUE` or `FALSE`) or the result of a logical operation or expression within your algorithm, you use a variable declared with the Boolean data type.

**Example**

```
$operationFlag AS Boolean <- TRUE
$pauseAvailable AS Boolean <- FALSE

NOTIFY {{the condition of the operation is $operationFlag}}    // TRUE
NOTIFY {{pause availability is $pauseAvailable}}  // FALSE
```

Here:

- `$operationFlag` is a Boolean variable initialized with `TRUE`.

- `$pauseAvailable` is a Boolean variable initialized with `FALSE`.

- `NOTIFY` informs the value of each variable.




## Operator Precedence and Parentheses

When combining multiple logical operators in a single expression, AlgoDraft follows a specific order of operations (precedence):

1. **NOT** has the highest precedence.
    
2. **AND** has the next highest precedence.
    
3. **XOR** has the next highest precedence.
    
4. **OR** has the lowest precedence.
    

Note: Define the exact precedence clearly in AlgoDraft's documentation. This order (NOT > AND > XOR > OR) is common but verify/specify for AlgoDraft.

This means NOT operations are evaluated first, then AND, then XOR, then OR.