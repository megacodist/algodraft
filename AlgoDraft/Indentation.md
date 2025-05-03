---
status: Draft
---
---
Indentation can be used or expressions and single-clause (non-multiline) statements

---

Indentation visually represents the structure and nesting of your logic. It makes code drastically easier to read and understand by grouping related instructions. We use indentation in two scenarios:

1.	Blocks
2.	Continuation

In this version of pseudocode, we use 3-space indentation but it is NOT a holy number.

# Indenting Inside Blocks
Any sequence of instructions that is logically contained within or controlled by another statement (forming a block) should be indented relative to that controlling statement. Every time you start a new block (inside `IF`, `WHILE`, `FUNCTION`, etc.), indent by exactly 3 spaces relative to the line that started the block. When a block ends (`ENDIF`, `ENDWHILE`, `ENDFUNCTION`, etc), the `END` keyword should align vertically with the keyword that started the block (`IF`, `WHILE`, `FUNCTION`, etc.).

```
// Level 0
FUNCTION CalculateGrade RETURNS String
   // Level 1
   PARAMS
      // Level 2
      $score AS Integer
   ENDPARAMS
   // Level 1 (Inside FUNCTION)
   $grade AS String

   IF $score >= 90 THEN
      // Level 2 (Inside IF)
      $grade <- "A"
   ELSE IF $score >= 80 THEN
      // Level 2 (Inside ELSE IF)
      $grade <- "B"
   ELSE
      // Level 2 (Inside ELSE)
      $grade <- "C" // Assuming C is lowest for simplicity
      // Still Level 2
      LogLowScore($score) // Another instruction in the ELSE block
   ENDIF // Aligned with IF/ELSE IF/ELSE

   // Level 1 (Back inside FUNCTION)
   RETURN $grade
ENDFUNCTION // Aligned with FUNCTION

// Level 0
$studentScore AS Integer
$finalGrade AS String
// Level 0
$studentScore <- 85
$finalGrade <- CalculateGrade($studentScore) // Function call is on level 0
NOTIFY {{student grade is $finalGrade}}
```

# Indenting Line Continuations
When a single logical instruction or statement is too long to fit comfortably on one line, subsequent lines belonging to that same instruction should be indented. Every time you continue a long line onto the next, you should indent the continuation line or lines by three spaces.

```
$longVariableName <- CalculateValue($param1, $param2) +
   AnotherFunctionCall($param3, $param4) - $someOffset

IF ($conditionA AND $conditionB) OR
   ($conditionC AND $conditionD) THEN
   // Instructions inside IF...
ENDIF
```
