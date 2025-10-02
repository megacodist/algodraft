---
status: Writing
---
The `Bits` basic type models an imaginary fixed-width span of the conceptual memory. So, the end user is working with bits, so `Bits`.

Each `Bits` object stores bits indexed from `[0 .. len-1)` where index `0` is the LSB (least significant bit), and index `len-1` is the MSB (most significant bit).

The width is fixed. So, any operation which changes width returns a new `Bits` object. The bits inside of this fixed span are mutable. So, operations that does not require new width, changes bits of the object.

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
	len AS Integer <- LENGTH OF this;
	IF len = 0 THEN
		RETURN; // No need to do anything
	ENDIF
	// Skip low bits until first 1
	indices AS IIterator<Integer> <- ITERATOR OF [0 .. len);
	FOR EACH idx IN indices DO
		IF this[idx] = TRUE THEN
			BREAK;
		ENDIF
	ENDFOR
	// Invert the rest
	FOR EACH idx IN indices DO
		this[idx] <- NOT this[idx];
	ENDFOR
ENDOPERATOR
```

### Binary `AND`, `OR`, and `XOR`

Creates a temporary object by MSB-extending the shorter operand with MSB, and then returns an object with bit-by-bit AND. The same goes for `OR` and `XOR`.

```AlgoDraft
OPERATOR this AND (pther AS Bits) -> Bits :=
	lenThis AS Integer <- LENGTH OF this;
	lenOther AS Integer <- LENGTH OF other;
	len AS Integer <- lenThis;
	a AS Bits <- this;
	b AS Bits <- other;
	IF lenThis > lenOther THEN
		len <- lenThis;
		b <- EXTEND other << (lenThis - lenOther) WITH MSB;
	ELSE IF lenOther > lenThis THEN
		len <- lenOther;
		a <- EXTEND this << (lenOther - lenThis) WITH MSB;
	ENDIF
	result <- DEEP COPY a;
	FOR EACH idx IN [0 .. len) DO
		result[idx] <- a[idx] AND b[idx];
	ENDFOR
	return result;
ENDOPERATOR
```

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
		RETURN this;
	ELSE
		msbIdx AS Integer <- len - 1;
		idx AS Integer <- msbIdx - 1;
		WHILE (idx >= 0) AND (this[msbIdx] = this[idx]) DO
			idx <- idx - 1;
		ENDWHILE
		idx <- idx + 1;
		RETURN this[0 .. idx];
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




```

// Interprets a bitstring as an unsigned integer andd converts it to its
// equivalent integer.
// Exceptions:
//  * `bad bitstring`: the string contains characters other than `"0"`
//      and `"1"`. 
FUNCTION str_b2_to_uint(bits AS String) -> Integer :=
	place, value AS Integer;
    IF bits IS EMPTY THEN
        RAISE {{bad argument: empty string is not allowed}}
    ENDIF
	value <- 0;
	FOR EACH idx IN [0 .. LENGTH OF bits) DO
        value <- value << 1;
		IF bits[idx] = "1" THEN
			value <- value OR 1;
        ELSE IF bits[idx] != "0" THEN
            RAISE {{bad argument: expected '0' or '1'}};
		ENDIF
	ENDFOR
	RETURN value;
ENDFUNCTION


// It may truncate the result if `len` is less than the needed length.
FUNCTION uint_to_str_b2(
        value AS Integer,
        len AS Integer OR NULL <- NULL,
        ) -> String :=
    IF value < 0 THEN
        RAISE {{bad argument: expected a nonnegative integer}}
    ENDIF
    bits AS List<Char> <- NEW List();
    IF len IS AN Integer THEN
        IF len <= 0 THEN
            RAISE {{bad argument: expected positive integer but got
                @len}}
        ENDIF
        FOR EACH i IN [0 .. len) DO
            IF (value AND 1) = 0 THEN
                APPEND "0" TO bits;
            ELSE
                APPEND "1" TO bits;
            ENDIF
            value <- value >> 1;
        ENDFOR
    ELSE IF len = NULL THEN
        WHILE value > 0 DO
            IF (value AND 1) = 0 THEN
                APPEND "0" TO bits;
            ELSE
                APPEND "1" TO bits;
            ENDIF
            value <- value >> 1;
        ENDWHILE
        IF bits IS EMPTY THEN
            APPEND "0" TO bits;
        ENDIF
    ELSE
        RAISE {{bad argument type: expected null or Integer but got 
            @(TYPE OF len)}}
    ENDIF
    DO {{Reverse the elements of @bits}};
    RETURN {{Create a string from the characters of @bits}};
ENDFUNCTION


// Interprets the provided bitstring as a signed integer and converts it
// to its equivalent integer.
// Exceptions:
//  * `bad bitstring`: the string contains characters other than `"0"`
//      and `"1"`. 
FUNCTION str_b2_to_int(bits AS String) -> Integer :=
    len AS Integer <- LENGTH OF bits;
    unsigned AS Integer <- str_b2_to_uint(bits);
    IF bits[0] = "0" THEN
        RETURN unsigned;
    ELSE
        RETURN unsigned - (1 << len);
    ENDIF
ENDFUNCTION



// It may truncate the result if `len` is less than the needed length.
FUNCTION int_to_str_b2(
        value AS Integer,
        len AS Integer OR NULL <- NULL,
        ) -> String :=
    isNegative AS Boolean;
    result AS String; 
    IF value < 0 THEN
        isNegative <- TRUE;
        result <- uint_to_str_b2(-value);
    ELSE
        isNegative <- FALSE;
        result <- uint_to_str_b2(value);
    ENDIF
    IF isNegative THEN
        result <- twos_complement_str(result);
    ENDIF
    resLen AS <- LENGTH OF result;
    IF len = NULL THEN
        RETURN result;
    ELSE IF len IS AN Integer THEN
        IF len > resLen THEN
            RETURN sign_extend(result, len);
        ELSE
            RETURN result[(resLen - len) .. resLen);
        ENDIF
    ELSE
        RAISE {{bad argument type: expected null or Integer but got 
            @(TYPE OF len)}};
    ENDIF
ENDFUNCTION


FUNCTION twos_complement_int(bits AS Integer) -> Integer :=
    RETURN (NOT int) + 1;
ENDFUNCTION


FUNCTION twos_complement_str(bits AS String) -> String :=
    result AS String;
    idxIter AS IIterator<Integer>;
    idx AS Integer;
    result <- DEEP COPY bits;
    idxIter <- ITERATOR OF ((LENGTH OF bits) .. 0];
    FOR EACH idx IN idxIter DO
        IF bits[idx] = "1" THEN
            BREAK;
        ENDIF
    ENDFOR
    FOR EACH idx IN idxIter DO
        result[IDX] <- "0" IF bits[idx] = "1" ELSE "1";
    ENDFOR
    RETURN result;
ENDFUNCTION


FUNCTION normalize(bits AS String) -> String :=
	len AS Integer <- LENGTH OF bits;
	IF len = 0 THEN
		RAISE {{bad string: no normalization for the empty string}};
	ELSE IF len = 1 THEN
		RETURN bits;
	ELSE IF bits[0] != bits[1] THEN
		RETURN bits;
	ELSE
		WHILE (len > 1) AND (bits[0] = bits[1]) DO
			bits <- bits[1 .. len);
			len <- len - 1;
		ENDWHILE
        RETURN bits;
	ENDIF
ENDFUNCTION


FUNCTION double_sign(bits AS String) -> String :=
	normalized AS String <- normalize(bits);
    RETURN normalized[0] + normalized;
ENDFUNCTION


FUNCTION sign_extend(
        bits AS String,
        new_length AS Integer,
        ) -> String :=
    len AS Integer <- LENGTH OF bits;
    IF len = 0 THEN
        RAISE {{bad string: no sign-extension for empty string}};
    ENDIF
    extra AS Integer <- new_length - len;
    IF extra < 0 THEN
        RAISE {{bad length: @bits is already longer than @new_length}};
    ELSE IF extra = 0 THEN
        RETURN bits;
    ENDIF
    extension AS String <- DO {{Create a string containing @extra number
        of @bits[0]}};
    RETURN extension + bits;
ENDFUNCTION


FUNCTION negate(bits AS String) -> String :=
    temp AS String;
    temp <- double_sign(bits);
    temp <- twos_complement_str(temp);
    temp <- normalize(temp);
    RETURN temp;
ENDFUNCTION


FUNCTION add(a AS String, b AS String) -> String :=
    lenA, lenB, len AS Integer;
    intA, intB, sum AS Integer;
    a <- double_sign(a);
    b <- double_sign(b);
    lenA <- LENGTH OF a;
    lenB <- LENGTH OF b;
    IF lenA > lenB THEN
        len <- lenA;
        b <- sign_extend(b, lenA);
    ELSE
        len <- lenB;
        a <- sign_extend(a, lenB);
    ENDIF
    intA <- str_b2_to_int(a);
    intB <- str_b2_to_int(b);
    sum <- intA + intB;
    RETURN int_to_str_b2(sum, len);
ENDFUNCTION


FUNCTION subtract(a AS String, b AS String) -> String :=
    RETURN add(a, negate(b));
ENDFUNCTION
```