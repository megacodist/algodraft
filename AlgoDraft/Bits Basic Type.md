---
status: Writing
---
0-based indexable of a fixed-width span of conceptual memory. So, any operation that preserve the width, can mutate the data in place, any operations that change the width, construct and return a new `Bits` object.

This type is indexable and indices starts from 0 (LSB) to MSB.

**Syntax**:

```AlgoDraft
INTERFACE IIterable<T> :=
    OPERATOR ITERATOR OF this -> IIterator<T>; // IIterator is the basic iterator type
ENDINTERFACE


INTERFACE IContainer<T> INHERITS IIterable<T> :=
    // Operator to check if an element is present within the container.
    // Relies on the container being IIterable (inherited contract) and its elements
    // (or the element being checked) supporting an equality comparison.
    OPERATOR (element AS T) IN this -> Boolean;
	
    // Operator to get the total number of elements in the container.
    OPERATOR LENGTH OF this -> Integer;
	
    // OPERATOR ITERATOR OF this -> Iterator<T>; is inherited from IIterable.
ENDINTERFACE


INTERFACE IROIndexable<T> INHERITS IContainer<T> :=
    // Adds read-only indexed access
    OPERATOR this[index AS Integer] -> T;
    // Inherits: LENGTH OF, IN, ITERATOR OF (from IContainer)
ENDINTERFACE


INTERFACE IRWIndexable<T> INHERITS IROIndexable<T> :=
    // Adds write access at a specific index
    OPERATOR (this[index AS Integer] <- (value AS T)) -> NULL;
    // Inherits: read this[index], LENGTH OF, IN, ITERATOR OF (from IROIndexable & IContainer)
ENDINTERFACE


INTERFACE IROSequentiable<T> INHERITS IROIndexable<T> :=
    // Adds slicing capability, returning a new instance of the same type as 'this'.
    OPERATOR this[interval AS Interval] -> IROSequentiable<T>;
    
    // Inherits: read this[index], LENGTH OF, IN, ITERATOR OF (from IROIndexable & IContainer)
ENDINTERFACE


CLASS Bits IMPLEMENTS IROSequentiable<Boolean> :=
    // Inherited from IContainer<T>. Not very useful.
	OPERATOR (bit AS Boolean) IN this -> Boolean;
	
    // Get the total number of elements in the container.
    OPERATOR LENGTH OF this -> Integer;
	
	// Get index, inherited from IROIndexable<T>
    OPERATOR this[index AS Integer] -> Boolean;
    
    // Set index, inherited from IRWIndexable<T>
    OPERATOR (this[index AS Integer] <- (bit AS Boolean)) -> NULL;
    
    // Slicing, inherited from IROSequentiable<T>
    OPERATOR this[interval AS Interval] -> IROSequentiable<Boolean>;
    
    // Iterators
    OPERATOR ITERATOR OF this -> Iterator<Boolean>; // Inherited from IIterable<T>
    OPERATOR LSB ITERATOR OF this -> IIterator<Boolean>;
    OPERATOR MSB ITERATOR OF this -> IIterator<Boolean>;
    
    // 
    OPERATOR NOT this -> NULL; // In-place
    OPERATOR TWOS COMPLEMENT this -> NULL;
    OPERATOR this AND (other AS Bits) -> Bits;
    OPERATOR this OR (other AS Bits) -> Bits;
    OPERATOR this XOR (other AS Bits) -> Bits;
    
    // 
    OPERATOR NORMALIZE this -> Bits;
    OPERATOR DOUBLE SIGN this -> Bits;
    
    // Shifts
    OPERATOR SHIFT this >> (n AS Integer) WITH MSB-> NULL;
    OPERATOR SHIFT this >> (n AS Integer) WITH 0 -> NULL;
    OPERATOR SHIFT this >> (n AS Integer) WITH 1 -> NULL;
    OPERATOR SHIFT this << (n AS Integer) WITH LSB -> NULL;
    OPERATOR SHIFT this << (n AS Integer) WITH 0 -> NULL;
    OPERATOR SHIFT this << (n AS Integer) WITH 1 -> NULL;
    
    // Rotates
    OPERATOR ROTATE this >> (n AS Integer) -> NULL;
    OPERATOR ROTATE this << (n AS Integer) -> NULL;
    
    // Extensions
    OPERATOR EXTEND this << (n AS Integer) WITH MSB -> Bits;
    OPERATOR EXTEND this << (n AS Integer) WITH 0 -> Bits;
    OPERATOR EXTEND this << (n AS Integer) WITH 1 -> Bits;
    OPERATOR EXTEND this >> (n AS Integer) WITH LSB -> Bits;
    OPERATOR EXTEND this >> (n AS Integer) WITH 0 -> Bits;
    OPERATOR EXTEND this >> (n AS Integer) WITH 1 -> Bits;
ENDCLASS
```

# Literals

## Bitstring

Like `0b01`, `0b1`, `0b001`.

## Hexastring

Like `0x02FD`, `0xab2`

# Operations

## Membership Test

`OPERATOR (bit AS Boolean) IN this -> Boolean;` inherited from `IContainer<T>`, though not very useful here.

## Width of the Block

`OPERATOR LENGTH OF this -> Integer;`, inherited from `IContainer<T>`, returns the fixed width of this span of the conceptual memory.

## Getting/Setting Bits Individually

```
// Get index, inherited from IROIndexable<T>
OPERATOR this[index AS Integer] -> Boolean;

// Set index, inherited from IRWIndexable<T>
OPERATOR (this[index AS Integer] <- (bit AS Boolean)) -> NULL;
```


## Iterators

```
OPERATOR ITERATOR OF this -> Iterator<Boolean>; // Inherited from IIterable<T>
OPERATOR LSB ITERATOR OF this -> IIterator<Boolean>;
OPERATOR MSB ITERATOR OF this -> IIterator<Boolean>;
```

`ITERATOR OF` and `LSB ITERATOR OF` are aliases to each other and return an iterator to iterate from LSB to MSB.

On the contrary, `MSB ITERATOR OF` returns an iterator to iterate from MSB to LSB.

## Basics

```
OPERATOR NOT this -> NULL; // In-place
OPERATOR TWOS COMPLEMENT this -> NULL;
OPERATOR this AND (other AS Bits) -> Bits;
OPERATOR this OR (other AS Bits) -> Bits;
OPERATOR this XOR (other AS Bits) -> Bits;
```

### `NOT`

Inverts bits.

### `TWOS COMPLEMENT`

Turns this bits into its two's complement representation.

```
OPERATOR TWOS COMPLEMENT this -> NULL :=
	indices AS IIterator<Integer> <- ITERATOR OF [0 .. (LENGTH OF this));
	FOR EACH idx IN indices DO
		IF this[idx] = TRUE THEN
			BREAK;
		ENDIF
	ENDFOR
	FOR EACH idx IN indices DO
		this[idx] <- NOT this[idx];
	ENDFOR
ENDOPERATOR
```

### Binary `AND`, `OR`, and `XOR`

Creates a temporary object by MSB-extending the shorter operand with MSB, and then returns an object with bit-by-bit AND. The same goes for `OR` and `XOR`.

## The Normalized Form

A `Bits` object is in **normalized (single-sign) form** if:

- it is non-empty, and
    
- either it has length `1`, or its first two MSBs, the two leftmost bits, differ.

```AlgoDraft
OPERATOR NORMALIZE this -> Bits :=
	len AS Integer <- LENGTH OF this;
	IF len = 0 THEN
		RAISE {{no normalized form for the empty}};
	ELSE IF len = 1 THEN
		RETURN bits;
	ELSE IF bits[len - 1] != bits[len - 2] THEN
		RETURN bits;
	ELSE
		lastIdx AS Integer <- len - 1;
		idx AS Integer <- lastIdx - 1;
		WHILE (idx >= 0) AND (this[lastIdx] = bits[idx]) DO
			idx <- idx - 1;
		ENDWHILE
		RETURN this[0 .. (idx + 1)];
	ENDIF
ENDOPERATOR
```

Equivalently, canonical form is the result of `normalize(bits)`. Canonical form contains **exactly one sign bit** (except length-1 strings where the single bit is the sign bit).

Examples:

- `normalize("0010") -> "010"`
    
- `normalize("1110") -> "110" -> "10"`
    
- `normalize("0") -> "0"`

## Double-sign form (exactly two sign bits)

A bitstring is in **double-sign form** if:

- its length ≥ 2, and
    
- the first two MSBs, the two leftmost bits, are equal, and
    
- if length > 2 then the third MSB bit is different from the sign bit.

In other words: double-sign form has **exactly two leading sign bits** and then a differing bit (unless total length = 2).

```AlgoDraft
OPERATOR DOUBLE SIGN this -> Bits :=
	temp AS Bits <- NORMALIZE this;
	RETURN (EXTEND temp << BY 1 WITH MSB);
ENDOPERATOR
```

**Important usage rule:** Double-sign form is a **temporary, pre-operation mode** you put operands into before negation/add/subtract. It is _not_ required to be preserved after you sign-extend operands to a shared operation width — converting to double-sign is a guard, not a storage invariant. It is a great protection against the tricky edge case of two's complement or to distinguish between sign overflow or value overflow.

## Shifts

Shifts are in-place operators.

`SHIFT <bits_obj> >> <num> WITH <bit>;`: Shifts the `Bits` object to the right `<num>` places, fills empty left places with `<bit>` which can be `MSB`, `0`, or `1`.

`SHIFT <bits_obj> << <num> WITH <bit>`: Shifts the `Bits` object to the left `<num>` places, fills empty left places with `<bit>` which can be `LSB`, `0`, or `1`.

## Rotates

Rotates are in-place operators.

`ROTATE <bits_obj> >> <num> ;`: Rotates the `Bits` object `<num>` places to the right.

`ROTATE <bits_obj> << <num> ;`: Rotates the `Bits` object `<num>` places to the left.

## Extensions

`EXTEND <bits_obj> << <num> WITH <bit>;`: Extend `<bits_obj>` to the left by adding `<num>` number `<bit>` which can be `MSB` (sign bit), `1` or `0`;

`EXTEND <bits_obj> >> <num> WITH <bit>;`: Extend `<bits_obj>` to the right by adding `<num>` number `<bit>` which can be `LSB`, `1` or `0`;

## Slicing

`<bits_obj>[intval AS Interval]`: Returns a newly-created `Bits` object with the specified indices as provided by the `Interval` object.

