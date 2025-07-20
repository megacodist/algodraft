---
status: Writing
---
# `CLASS`

How about replacing `NEW` for `CONSTRUCTOR` in a class for constructor definition?

# Casting

What is casting and how should it be implemented?

Good syntax? `CAST obj TO DataType`

# Downcasing

One of the use cases of `IS A/AN` operator mentioned in AlgoDraft is downcasing. Please explain it more.

# Index Basic Type

How about adding a basic type named `Index` very like `Integer` but contains two special values `FIRST` and `LAST` to work with indexable data structures.

```
lines AS Tuple<String...> <- INPUT {{some lines, might start and end with '*'}}
// Removing possible comments (starting with *)...
startIdx AS Index <- FIRST + 1 IF {{@lines[FIRST] starts with '*'}}	ELSE FIRST
stopIdx AS Index <- LAST - 1 IF {{@lines[LAST] starts with '*'}} ELSE LAST
lines <- lines[startIdx .. stopIdx]
```

# `IS A/AN`

Right now, this operator only works on objects as follow:

```
<obj> IS A/AN <type_interface>
```

`<obj>` is an object. How about overloading this operator by allowing the left-hand operator to be both an object or a type object.

# IContainer

How about renaming `IContainer` to `IContainerable`?

# IIterable Interface

How about making the syntax generic (adding `<T>`)?

```
INTERFACE IIterable<T> :=
    OPERATOR ITERATOR OF this -> Iterator<T> ENDOPERATOR
ENDINTERFACE
```

# Mapping

There are two mapping annotations:
1. `Mapping<KT TO VT>`
2. `Mapping<KT -> VT>`

# Numeric Basic Type

I decided to have `±INFINITY`/`±∞` conceptual values in numeric types. So, their possible operations must be defined in `Numeric` class.

# RECORD

How about a one-line (or sometimes inline) version of RECORD syntax.

We believe that distinction between `RECORD`s and `CLASS`es are not very good. A simple data aggregate (`RECORD`) needs comparing for equality (`=`) and this means operator definition in a `RECORD` which by default is forbidden. So it is a good idea to gather all user-defined types under the same umbrella of CLASS`.

# Sequence Slicing

Changing the slicing syntax to `[a .. b Δ c]` must be considered.
How about the following open-ended notations? `[.. b]` and `[a ..]`

# Sequences vs Indexables

Sequences introduce the following extensions to their indexables counterparts:

1. **Comparability**: They implement `IComparable` with the default comparing behavior of lexicographical order:
2. **Slicing**

```
INTERFACE IROSequentiable INHERITS IROIndexable, IComparable :=
    OPERATOR this[interval AS ISteppedIntStream] -> IROSequentiable
ENDINTERFACE
```

# Sigil

The preceding character in variable and constant names are called sigil. In this proposed syntax:

- **Variables:** Remove `$` from declaration and usage examples. Syntax becomes `varName AS DataType [<- initialValue]`. Explain that variables are now plain identifiers. 

- **Constants:** Remove `$` from declaration and usage examples. Syntax becomes `CONST CONST_NAME AS DataType <- value`. Explain that constants are now plain identifiers (typically all caps by convention).

- Explain that all AlgoDraft identifiers (variables, constants, type names, function names, interface names, etc.) when referred to within an NLD **must** be prefixed with `@`. 
- Provide clear examples: `NOTIFY {{user @userName (type: @UserRecord) logged in using @LoginFunction}}`.
- Reiterate that outside NLDs, these identifiers are used without the `@`.
- Update "Key Characteristics of NLDs" to reflect this: "Integration of Identifiers: NLDs explicitly reference AlgoDraft identifiers by prefixing them with `@` (e.g., `@userCount,` `@ProcessData`, `@ErrorType`)."
- Update "NLDs Best Practices" if needed.

# Tuple

Variadic syntax of tuple type annotation is start-anchored: `Tuple<Type1, Type2, ..., TrailingType...>`. 

How about defining:

* End-anchored syntax like `Tuple<LeadingType..., Type1, Type2>`.

* Both-end-anchored syntax like `Tuple<Type1, MiddleType..., Type2>`.

* Double (or even multiple) variadic syntax like `Tuple<Integer..., String...>`

