---
status: Draft
---
Variables are named containers used to store data that might change during the execution of your algorithm. Think of them as labeled boxes where you can put information and update it later. Before you can use a variable, you must declare it.

# Variable Declaration Syntax
Declaration specifies the variable's name and the type of data it will hold:
```
$variableName AS DataType <- <value/literal>
```

* **`$`**: All variable names must start with a dollar sign ($).

* **`variableName`**: The name you choose for the variable. By convention, use [[Appendix 2, Naming conventions#camelCase|camelCase]] (e.g., `userAge`, `firstName`, `totalScore`).

* **`AS DataType`**: Specifies the type of data the variable can store (e.g., `Integer`, `String`, `Boolean`, `List<Real>`). This is mandatory.
* **`<- <value/literal>`** (Optional): The optional assignment operator.

**Example**

```
$counter AS Integer
$age AS Integer <- 40
$customerId AS String
$customerName AS String <- "Megacodist"
$isActive AS Boolean
```
