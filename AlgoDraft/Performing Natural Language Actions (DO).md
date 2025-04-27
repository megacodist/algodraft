---
status: Draft
---
Sometimes we need to describe an action rather than imperatively coding it. In such cases we use **Natural Language Statements** (NLSes). The action:

* usually involves complexity better described in natural language than with simple operators.

* must be expressed via an NLD employing imperative language.

* either results in a value that needs to be assigned to a variable or causes side effect.

**Syntax (Action-Only)**
```
DO {{imperative_nld_sentence}}
```

```
DO {{sort $mylist}}
DO {{add 3 to $idx}}
```

**Syntax (Value-Yielding)**
```
$variable AS DataType <- DO {{imperative_nld_sentence}}
```

```
$flag AS Boolean <- DO {{check that $list items are in acsending order}}
$settFile AS File <- DO {{open settings file in read/write, binary mode}}
```
