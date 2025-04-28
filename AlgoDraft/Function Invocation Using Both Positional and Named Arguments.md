---
status: Draft
---
You can combine both styles in a single function call for flexibility, that is provide some initial arguments positionally, then switch to named arguments for the remainder.

**Positional-before-named rule**:
All positional arguments **MUST** come **BEFORE** all named arguments. You cannot place a positional argument after a named argument in the same call.

**Syntax**:
```
DoSomething(posArg1, posArg2, ..., $namedArg1 <- valueN, $namedArg2 <- valueM, ...)
```

**Pros:**
- Offers flexibility - use concise positional style for required initial parameters and clear named style for optional or later parameters.
    
**Cons:** 
- Requires adherence to the positional-before-named rule.

**Example**:

```
FUNCTION Connect
    PARAMS
        IN $host AS String             // Required parameter
        IN $port AS Integer <- 8080    // Optional parameter, defaults to 8080
        IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
    ENDPARAMS
    // ... function body ...
ENDFUNCTION


// VALID Mixed Calls:
Connect("main.server.com", $timeout <- 15000)
// $host gets "main.server.com" (positional)
// $port uses default 8080 (skipped positionally, not specified by name)
// $timeout gets 15000 (named)

Connect("backup.server", $port <- 8081, $timeout <- 2000)
// $host gets "backup.server" (positional)
// $port gets 8081 (named)
// $timeout gets 2000 (named)

// INVALID Mixed Call:
// Connect($port <- 9000, "fails.com") // ERROR: Positional argument after named argument
```
