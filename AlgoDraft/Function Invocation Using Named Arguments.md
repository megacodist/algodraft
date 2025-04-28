---
status: Draft
---
This method explicitly matches arguments to parameters by name, making calls clearer and more robust. Arguments are specified using the parameter name (including the `$`), followed by assignment operator (`<-`), and then the value/variable. The order in which named arguments are listed does not matter.

**Syntax**:

```
DoSomething($param1Name <- value1, $param2Name <- value2, ...)
```

**Pros:**
- Highly readable and self-documenting.
- Order independence prevents errors.
- Easy to specify only certain optional arguments regardless of their position.

**Cons:**
- More verbose than positional arguments.

**Example 1:**
```
FUNCTION Calculate RETURNS Integer
    PARAMS
        IN $a AS Integer
        IN $b AS Integer
        IN $op AS String <- "+"
    ENDPARAMS
	
	 // Body
ENDFUNCTION


$result AS Integer
$result <- Calculate($a <- 10, $b <- 5)              // $op uses default "+"
$result <- Calculate($b <- 7, $a <- 20, $op <- "*")  // Order doesn't matter
$result <- Calculate($a <- 15, $op <- "/", $b <- 3)  // Order doesn't matter
```

**Example 2:**
```
FUNCTION Connect
    PARAMS
        IN $host AS String             // Required parameter
        IN $port AS Integer <- 8080    // Optional parameter, defaults to 8080
        IN $timeout AS Integer <- 5000 // Optional parameter, defaults to 5000ms
    ENDPARAMS
    
    // ... function body ...
ENDFUNCTION


Connect($host <- "api.server.net") // $port and $timeout use defaults
Connect($host <- "db.server.local", $timeout <- 10000) // $port uses default
Connect($port <- 9001, $host <- "backup.server")     // $timeout uses default, order doesn't matter
```
