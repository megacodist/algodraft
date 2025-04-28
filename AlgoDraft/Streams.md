---
status: Draft
---
Beyond regular functions that compute a single result or perform a distinct action, AlgoDraft supports **Streams**. Streams are used to define routines that can **yield** a sequence of values over time based on initial configuration or internal logic, producing each value only when requested (lazy generation).

**Syntax:**
The definition structure of streams shares similarities with function definitions regarding the overall block structure and parameter list.

```
STREAM StreamName YIELDS YieldType // MANDATORY: Declares the type of items yielded
    PARAMS
        $param1 AS DataType1       // Parameter definitions
        IN $param2 AS DataType2 <- defaultValue
        // ... Only IN parameters are allowed for Streams ...
    ENDPARAMS
	
	// Body: Contains logic, local variables, and YIELD statements
	// ... Code executes incrementally ...
	
	YIELD valueOfTypeYieldType // Produces the next value and pauses
	
	// ... More code ...
	
	RAISE EXHAUSTION // Optional: Manually terminate the stream
	
	// ... More code ...
	
	// Reaching ENDSTREAM or a bare RETURN also terminates the stream
ENDSTREAM
```

**Notes:**

Only `IN` [Parameter Directionality](#parameter-directionality-in-out-inout) is permitted for parameters defined within a stream's `PARAMS` block. The multi-stage, suspend/resume nature of streams makes the semantics of `OUT` and `INOUT` parameters (which typically relate to the single completion state of a routine) complex and counter-intuitive in this context. Inputs configure the stream's generation process; outputs are exclusively handled via `YIELD`.

#### Semantics: Lazy Evaluation and State

The behavior of streams differs significantly from regular functions:

1. **Stream Object Creation**: Calling a stream definition (e.g., `myStream <- StreamName(arg1, ...)`) does not execute the code inside the `STREAM` body immediately. Instead, it creates and returns a special [`Stream` object](#stream-data-type). This object represents the paused, ready-to-run generator and encapsulates its internal state.
2. **Lazy Execution**: The code inside the stream body only starts running when the first request for a value is made from its `Stream` object.
3. **`YIELD` Keyword**: When the `YIELD $value` statement is encountered inside the stream's body:
   * The stream's execution is paused immediately after the `YIELD`.
   * The specified value (which must match the `YieldType`) is sent back as the result of the request that resumed the stream.
   * The stream's internal state is preserved.
4. **Resumption**: The next request for a value will resume the stream's execution right after the `YIELD` statement where it last paused, with its preserved state intact.
5. **Exhaustion**: The stream becomes "exhausted" (finished) when:
   * It reaches the `ENDSTREAM` keyword naturally.
   * A bare `RETURN` statement is executed within its body.
   * A `RAISE EXHAUSTION` statement is explicitly executed.

