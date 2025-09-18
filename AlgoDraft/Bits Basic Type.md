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


CLASS Bits IMPLEMENTS IRWIndexable<Boolean> :=
    // Inherited from IContainer<T>. Not very useful.
	OPERATOR (bit AS Boolean) IN this -> Boolean;
	
    // Get the total number of elements in the container.
    OPERATOR LENGTH OF this -> Integer;
	
	// Get index, inherited from IROIndexable<T>
    OPERATOR this[index AS Integer] -> Boolean;
    
    // Set index, inherited from IRWIndexable<T>
    OPERATOR (this[index AS Integer] <- (bit AS Boolean)) -> NULL;
    
    // Iterators
    OPERATOR ITERATOR OF this -> Iterator<Boolean>; // Inherited from IIterable<T>
    OPERATOR LSB ITERATOR OF this -> IIterator<Boolean>;
    OPERATOR MSB ITERATOR OF this -> IIterator<Boolean>;
    
    // 
    OPERATOR NOT this -> NULL; // In-place
    OPERATOR this AND (other AS Bits) -> Bits;
    OPERATOR this OR (other AS Bits) -> Bits;
    OPERATOR this XOR (other AS Bits) -> Bits;
    OPERATOR TWOS COMPLEMENT this -> Bits;
    
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
    
    // Cuts
    OPERATOR LSB CUT this BY (n AS Integer) -> Bits;
    OPERATOR MSB CUT this BY (n AS Integer) -> Bits;
    
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
OPERATOR TWOS COMPLEMENT this -> Bits;
OPERATOR this AND (other AS Bits) -> Bits;
OPERATOR this OR (other AS Bits) -> Bits;
OPERATOR this XOR (other AS Bits) -> Bits;
```

### `NOT`

Inverts bits.

### Binary `AND`, `OR`, and `XOR`

Creates a temporary object by MSB-extending the shorter operand with 0, and then returns an object with bit-by-bit AND. The same goes for `OR` and `XOR`.



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

// Cuts
OPERATOR LSB CUT this BY (n AS Integer) -> Bits;
OPERATOR MSB CUT this BY (n AS Integer) -> Bits;

// Extensions
OPERATOR EXTEND this << (n AS Integer) WITH MSB -> Bits;
OPERATOR EXTEND this << (n AS Integer) WITH 0 -> Bits;
OPERATOR EXTEND this << (n AS Integer) WITH 1 -> Bits;
OPERATOR EXTEND this >> (n AS Integer) WITH LSB -> Bits;
OPERATOR EXTEND this >> (n AS Integer) WITH 0 -> Bits;
OPERATOR EXTEND this >> (n AS Integer) WITH 1 -> Bits;














# Syntax

A **binary literal**, also called **bitstring**, starts with `0b` and is followed by a run of bits (0/1). The lexer grabs the whole run; you cannot split a `0b1010` mid-run — it's one literal. The first bit is the sign bit for _that literal width_.

A binary literal token is defined as:

```
BINARY_LITERAL := "0b" BITS
BITS           := "0" | "1" , { "0" | "1" }   # one or more bits, greedy
```

**Lexer rule (deterministic):** after `0b` read the maximal contiguous run of `0`/`1` characters as a single token. Tokens must be separated by whitespace, punctuation, or operators (i.e., `0b10 0b100` OK, `0b1000b1` is two tokens only if separated).

Bits are MSB → LSB (leftmost bit is the most significant bit; and rightmost, the least significant bit).

# Semantics, Mapping bits to integer

## The Sign Bit

The leftmost bit (the MSB) is also **sign bit**:

* If the sign bit is `0`, it’s a nonnegative literal;
* Otherwise, if `1`, it’s a negative literal in two’s complement for the width given by the number of bits present.

The sign bit can be repeated without changing the value associated to the literals. If it is more than one, the extra is called **leading sign bit/bits**, specifically **leading zero/zeros** for positive and **leading one/ones** for negative literals. For example:

* `0b010 = 0b0010 = 0b00010 = 0b000010` etc.
* `0b10 = 0b110 = 0b1110 = 0b11110` etc.

## Canonical form (single-sign mode)

A bitstring is in **canonical form** if:

- it is non-empty, and
    
- either it has length `1`, or its first two MSBs, the two leftmost bits, differ (`bits[0] != bits[1]`).

```AlgoDraft
FUNCTION normalize(bits AS String) -> String :=
	len AS Integer <- LENGTH OF bits;
	IF len = 0 THEN
		RAISE {{no normalized form for the empty string}}
	ELSE IF len = 1 THEN
		RETURN bits;
	ELSE IF bits[0] != bits[1] THEN
		RETURN bits;
	ELSE
		WHILE (len > 1) AND (bits[0] = bits[1]) DO
			bits <- bits[1 .. len);
			len <- len - 1;
		ENDWHILE
	ENDIF
ENDFUNCTION
```

Equivalently, canonical form is the result of `normalize(bits)`. Canonical form contains **exactly one sign bit** (except length-1 strings where the single bit is the sign bit).

Examples:

- `normalize("0010") -> "010"`
    
- `normalize("1110") -> "110" -> "10"`
    
- `normalize("0") -> "0"`

## Double-sign form (exactly two sign bits)

A bitstring is in **double-sign form** if:

- its length ≥ 2, and
    
- the first two MSBs, the two leftmost bits, are equal (`bits[0] == bits[1]`), and
    
- if length > 2 then the third bit is different from the sign bit (`bits[2] != bits[0]`).

In other words: double-sign form has **exactly two leading sign bits** and then a differing bit (unless total length = 2).

```AlgoDraft
FUNCTION double_sign(bits AS String) -> String :=
	len AS Integer <- LENGTH OF bits;
	IF len = 0 THEN
		RAISE {{no double-sign bit form for the empty string}};
	ELSE IF len = 1 THEN
		RETURN bits + bits;
	ELSE IF bits[0] != bits[1] THEN
		RETURN bits[0] + bits;
	ELSE
		WHILE (len > 1) AND (bits[0] = bits[2]) DO
			bits <- bits[1 .. len);
			len <- len - 1;
		ENDWHILE
		RETURN bits;
	ENDIF
ENDFUNCTION
```

**Important usage rule:** Double-sign form is a **temporary, pre-operation mode** you put operands into before negation/add/subtract. It is _not_ required to be preserved after you sign-extend operands to a shared operation width — converting to double-sign is a guard, not a storage invariant.

## Positive values

A non-negative binary can be converted into an integer using the elementary algebra rules: adding the values of places holding ones:

```
FUNCTION str_b2_to_uint(bits AS String) -> Integer :=
	place, value AS Integer;
	value <- 0;
	place <- 1;
	FOR EACH idx IN (LENGTH OF bits .. 0] DO
		IF bits[idx] = "1" THEN
			value <- value + place;
		ENDIF
		place <- place << 1;
	ENDFOR
	RETURN value;
ENDFUNCTION
```

**Examples**:

| Binary Literal | Integer Equivalent |
| -------------- | ------------------ |
| `0b01`         | 1                  |
| `0b010`        | 2                  |
| `0b011`        | 3                  |
| `0b0110`       | 6                  |

## Two's Complement

The two's complement of a binary literal is computed in one of these two ways:

### Using Exponentiation

Follow these steps:

1. Interpret the bitstring as unsigned representation.
2. Subtract the yielded integer from `2 ** m` where `m` is the length of the original bitstring.
3. Convert the result to a bitstring of `m` bits.

```
FUNCTION twos_complement(bits AS String) -> String :=
    len AS Integer <- LENGTH OF bits;
    value AS Integer <- str_b2_to_uint(bits);
    value <- (1 << len) - value;
    RETURN {{Convert @value to a bitstring of @len bits.}};
ENDFUNCTION
```

### Using Negation/Increment

Follow these steps:

1. Invert all bits, zeros to ones and ones to zeros.
2. Add 1.

```
FUNCTION twos_complement(bits AS String) -> String :=
    len AS Integer <- LENGTH OF bits;
    value AS Integer <- str_b2_to_uint(bits);
    value <- (NOT value) + 1;
    RETURN {{Convert @value to a bitstring of @len bits.}};
ENDFUNCTION
```

### Using 

```

```

* Or, keep the literal from the LSB until the first `1` intact and invert bits just the first `1` up to the MSB, including sign bit/bits.

| Binary Literal | Two's Complement |
| -------------- | ---------------- |
| `0b01`         | `0b11`           |
| `0b1001`       | `0b0111`         |
| `0b01100`      | `0b10100`        |

## The Negation Rule

To convert a binary literal to its negation:

1. Convert the literal to double-sign form.
2. Compute the two's complement of it.
3. Convert the result to canonical form.

Long story short, the negation of a binary literal is the canonical form of the two's complement of its double-sign form.

# Addition

Add two binary literals as:

1. Sign-extend the longest operand into its complement form.
2. Sign-extend the shortest operand to conform to the other.
3. Add using regular algebra rules.
4. Ignore overflow if occurred.
5. Convert the result to its canonical form.

# Subtraction

Subtraction is the same as addition just negate the second operand beforehand.