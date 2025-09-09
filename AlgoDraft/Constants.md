---
status: Draft
---
Constants are named containers used to store data that will not change after it's initially defined. They are useful for fixed values like mathematical constants (Pi), configuration settings (maximum attempts), or status codes. Constants must be defined using the `CONST` keyword and must be assigned a value immediately.

#### Syntax:
```
CONST CONSTANT_NAME AS DataType <- <value/literal>
```
* **`CONST`**: The keyword indicating that this is a constant.

* **`CONSTANT_NAME`**: The name you choose for the constant. By convention, use [[Appendix 2, Naming conventions#SCREAMING_SNAKE_CASE, UPPER_SNAKE_CASE or MACRO_CASE|UPPER_SNAKE_CASE]] (e.g., `MAX_RETRIES`, `PI`, `DEFAULT_TIMEOUT`).

* **`AS DataType`**: Specifies the fixed data type.

* **`<- <value>`**: The assignment operator is used here only to set the constant's initial (and final) value. This value must be provided at definition time.

#### Example
```
CONST PI_MATH AS Real <- 3.14159
CONST $MAX_LOGIN_ATTEMPTS AS Integer <- 3
CONST $ADMIN_EMAIL AS String <- "admin@example.com"
```

#### Immutability

Once a constant is defined, you cannot change its value. Attempting to assign a new value to a constant is an error in concept.

```
CONST $MAX_USERS AS Integer <- 100

// The following would be conceptually WRONG in AlgoDraft:
// $MAX_USERS <- 101 // ERROR: Cannot assign to a constant
```

It is a good practice to join two or more parts using underscore in order to avoid confusion with keywords.
