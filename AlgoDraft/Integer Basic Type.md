---
status: Writing
---
# Constructor

```
int AS Integer <- Integer(data);
```

Tries to convert the data to an integer:
* `String`: `Integer("123")` 
* `Real`: `Integer(23.36)`
* `Bits`: `Integer("0b1100010")`

# Binary Representation

AlgoDrat recognizes no containing width, positive integers must start with `0` and negatives with `1`.  Negative numbers are shown using two's complement.

| Mathematical number | AlgoDraft Binary Representation |
| ------------------- | ------------------------------- |
| 4                   | 0b0100                          |
| -4                  | 0b100                           |

It is possible to repeat the leftmost binary digit any number. If the leftmost digit is repeated more than once, the extra would be unnecessary leading zeros (for positive integers) or ones (for negative integers):

| Mathematical number | AlgoDraft Binary Representation |
| ------------------- | ------------------------------- |
| 4                   | 0b0100 = 0b00100 = 0b000100     |
| -4                  | 0b100 = 0b1100 = 0b11100        |

For addition or subtraction, make their length equal, adding one more leading zero or one, and perform the result, and ignoring the overflow.

# Bitwise Operations

| Operator | Name | Example | Result |  
| :------- | :----------- | :------------- | :----- |  
| a & b | Bitwise AND | 12 & 10 | 8 |  
| a \| b | Bitwise OR | 12 \| 10 | 14 |  
| a ^ b | Bitwise XOR | 12 ^ 10 | 6 |  
| ~a | Bitwise NOT | ~12 | -13 |  
| a << b | Left Shift | 10 << 2 | 40 |  
| a >> b | Right Shift | 10 >> 1 | 5 |  
| a >> b | Right Shift | (-10) >> 1 | -5 |

## Critical Semantic Details You MUST Define

Whichever syntax you choose (and you should choose Option 1), the real design work is in defining the semantics.

1. **Bitwise NOT (~) Behavior:**
    
    - What is the "width" of an AlgoDraft Integer? Is it a fixed 32-bit or 64-bit value? Or is it an arbitrary-precision integer (like in Python)?
        
    - This is critical. ~ on a fixed-width integer is a simple bit flip. ~x is equivalent to (-x) - 1 in two's complement.
        
    - ~ on an arbitrary-precision integer is problematic. What is ~5 if there are infinite leading zero bits to flip to one? Python's ~5 is -6, which follows the two's complement rule, but it's a defined behavior, not a simple bit flip of an infinite string.
        
    - **Recommendation for AlgoDraft:** Define Integer as conceptually being a **64-bit two's complement integer** for the purpose of bitwise operations. This is a common, practical choice that gives designers a concrete model to work with.
        
2. **Right Shift (>>) Behavior for Signed Integers:**
    
    - **Arithmetic Shift (Common):** When a negative number is right-shifted, the sign bit (the most significant bit, which is 1) is copied to fill the new bits on the left. This preserves the number's sign.
        
        - (-8) >> 1 -> (-4)
            
    - **Logical Shift:** The new bits on the left are always filled with 0, regardless of the sign. This is equivalent to an unsigned division by a power of 2.
        
        - (-8) >> 1 would result in a very large positive number.
            
    - Some languages provide different operators for this (e.g., Java has >> for arithmetic and >>> for logical).
        
    - **Recommendation for AlgoDraft:** Define >> as an **arithmetic right shift** for Integers (assuming they are signed). This is the most common and expected behavior in C-family languages and Python.