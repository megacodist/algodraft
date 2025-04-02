# Introduction
Humans need to convey their ideas or exchange ideas with other people. If you want to talk about mathematics, you need to know algebra, set theory, their theoroms, gargons among other things. Pseudocode is the language of algorithms.

It is the general way to express algorithmic ideas.

It is worth mentioning that there is NOT a standard pseudocode that everyone agrees upon.

AlgoDraft is the result of our effort to have the most formal-informal algorithm language: the most formal among all casual contracts and the most informal among all programming langiages.

## Characteristics
Pseudocode ia all about logic not implementation.

It must be as higher level as possible if we do not want to say the highest level. The ligh level in programming sense means free from any limiting technology, assuming everything is possible.

pseudocode prioritizes clarity and simplicity over strict syntax.

Pseudocode's primary goal is clear communication of logic.

Pseudocode must aim at a broad audience or focusing on procedural steps, I would lean towards $foo OF TYPE Integer because its explicitness prioritizes immediate understanding over symbolic elegance.

With collaboration with Google Gemini, I (Megacodist) managed to outlined this pseudocode named AlgoDraft with file extension of .algod.

## Benefits
1.	Pseudocode is a way to think out of the box.
2.	It enables everyone to not limit your train of thoughts to any programming language or framework.
3.	It is possible to employ different levels of abstraction. For example, knowing a prefix in string manipulation is a substring starting from the beginning, we can have all of the following:
Leading 🡨 Obtain

# Data types
## Boolean
Variables of this type can hold only `TRUE` and `FALSE` values.

## Integer

## String
Strings are 0-based-indexed sequences of characters.

## Real

## FsPath
Represents a class to instantiate objects to work with file system items.

## File
An instance of this type represents an opened file ready for I/O operations.

### Methods
#### Opening

```
$configFile <- DO: open the file $filePath in text, read mode
```

#### Closing
```
DO: close $configFile
```

# Basic syntax
## Variables
All variables must start with `$` character and adhere to Camel Case convention.

We use `AS` keyword to specify the type of the variablb.

```
$someInt AS Integer
```