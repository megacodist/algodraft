---
status: Planned
---
Because of expanding and shrinking capabilities, lists can be thought of as superior to linked list and be employed where they are needed.
# Instantiation

```
tempList AS List<Integer> <- NEW List(20 TIMES 0) // Creating alist of twenty zeros
```

# Operations

## Expanding

### Appending, Prepending, and Inserting an Element

These are in-place operations to alter a list object.

**Syntax**:

```
// Appends a single element to the end of the list.
OPERATOR APPEND (elem AS T) TO this;

// Prepends a single element to the start of the list.
OPERATOR PREPEND (elem AS T) TO this;

// Inserts a single element at a specific index, shifting subsequent elements.
OPERATOR INSERT (elem AS T) TO this AT (index AS Integer);
```

### Extending with an Iterable

These are in-place operations to alter a list object.

**Syntax**:

```
// Appends all elements from an iterable to the end of the list.
OPERATOR APPEND ALL (elems AS IIterable<T>) TO this;

// Prepends all elements from an iterable to the start of the list.
OPERATOR PREPEND ALL (elems AS IIterable<T>) TO this;

// Inserts all elements from an iterable at a specific index.
OPERATOR INSERT (elems AS IIterable<T>) TO this AT (index AS Integer);
```

## Shrinking

### Deleting an index

```
OPERATOR DEL this[idx AS Integer] -> NULL;
```

### Deleting a range of indices

```
OPERATOR DEL this[indices AS IIntegerStream] -> NULL;
```
