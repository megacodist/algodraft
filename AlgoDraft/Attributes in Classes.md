---
status: Draft
---
Attributes are the variables defined within a class that hold the data, or **state**, for objects created from that class. They represent the characteristics or properties of the objects. They are fundamental to defining what an object "knows" or what properties it possesses.

**General Declaration Syntax for Attributes:**

```
[<visibility>] [STATIC] [CONST] $attributeName AS DataType [<- <initialValueForStatic>]
```

This syntax specifies the following characteristics for attributes:

1. **Accessibility**: Who can access the attributes?

2. **Mutability**: Is it possible to change their values?

3. **Ownership**: AlgoDraft distinguishes between two kinds of attributes:
	
	* **Class Attributes (Static Attributes):** Belong to the class as a whole, shared by all instances.
	
	* **Instance Attributes:** Belong to individual objects; each object has its own copy.

All attributes, whether class or instance, must be declared within the `CLASS ... ENDCLASS` block. It's a common convention to declare attributes before methods, typically grouping class attributes together first, followed by instance attributes.

# Class Attributes (Static Attributes)

Class attributes, also known as **static attributes**, belong to the **class itself**, rather than to any individual object (instance) created from that class. This means there is only **one single copy** of each class attribute, and this copy is shared among all instances of the class. You can even access class attributes using the class name directly, without needing to create an object first.

Class attributes are typically used for:

- Storing values that are common to all instances, such as default configurations or shared counters.

- Defining constants that are intrinsically linked to the class (e.g., mathematical constants within a `MathUtils` class, or maximum values).

**Declaration Syntax:**

Class attributes are declared using the `STATIC` keyword. They can be mutable or constant (`CONST`).

1. **Mutable Class Attribute:**
    
    ```
    [<visibility>] STATIC $camelCaseName AS DataType [<- <initialValue>]
    ```
    
2. **Constant Class Attribute:**
    
    ```
    [<visibility>] STATIC CONST $UPPER_SNAKE_CASE_NAME AS DataType <- <initialValue>
    ```

**Key Points for Declaration:**

* **`<visibility>`**: Optional [[Encapsulation|attribute visibility]] defaults to `PUBLIC`. Determines which client code can access the attribute.

- **`STATIC` Keyword:** This keyword signifies that the attribute belongs to the class, not to individual instances.

- **`CONST` Keyword (Optional):**
    
    - If `CONST` is present, the attribute's value cannot be changed after its initial assignment. It must be initialized at the point of declaration. The name (`$UPPER_SNAKE_CASE_NAME`) must be conformed to constants naming rules.
    
    - If `CONST` is absent, the attribute is mutable (its value can be changed during program execution). The name (`$camelCaseName`) must be conformed to variables naming rules.

- **`AS DataType`**: Standard AlgoDraft syntax for annotation and typing.

- **`<- <initialValue>` (Initialization):**
    
    - For `STATIC CONST` attributes, an initial value **must** be provided at declaration.
    
    - For mutable `STATIC` attributes, an initial value **can** be provided. If not, its value is undefined (random).

**Example:**

```
CLASS GameSessionManager
    // --- Class (Static) Attributes ---
    STATIC CONST $MAX_SESSIONS AS Integer <- 100 // Max allowed sessions (constant)
    STATIC $activeSessionCount AS Integer <- 0    // Current active sessions (mutable)

    // --- Instance Attributes ---
    // (e.g., $sessionId AS String, $playerName AS String - to be defined later)
    // ...

    // --- Constructor ---
    METHOD CONSTRUCTOR
        PARAMS
            // ... parameters for instance attributes ...
        ENDPARAMS
        
        IF GameSessionManager.$activeSessionCount < GameSessionManager.$MAX_SESSIONS THEN
            // ... initialize instance attributes ...
            GameSessionManager.$activeSessionCount <- GameSessionManager.$activeSessionCount + 1
            NOTIFY {{new session started and number of active sessions in GameSessionManager.$activeSessionCount}}
        ELSE
            NOTIFY {{maximum session limit (GameSessionManager.$MAX_SESSIONS) reached}}
            // Handle error - perhaps by not fully creating the session object
        ENDIF
    ENDMETHOD

    // --- Other Methods ---
    METHOD EndSession // Example method (could be STATIC or instance based on design)
        PARAMS
            // ... parameters if needed ...
        ENDPARAMS
        // Assuming a session is properly ended and we need to decrement the count
        IF GameSessionManager.$activeSessionCount > 0 THEN
            GameSessionManager.$activeSessionCount <- GameSessionManager.$activeSessionCount - 1
        ENDIF
        NOTIFY {{session ended and number of active sessions in GameSessionManager.$activeSessionCount}}
    ENDMETHOD

ENDCLASS

// --- Using the Class and its Static Attributes ---

NOTIFY {{maximum game sessions allowance in GameSessionManager.$MAX_SESSIONS}}
NOTIFY {{initially active sessions in GameSessionManager.$activeSessionCount}} // Will be 0

// Creating some session objects (constructor will increment $activeSessionCount)...
$session1 AS GameSessionManager <- NEW GameSessionManager( {{constructor args...}} )
$session2 AS GameSessionManager <- NEW GameSessionManager( {{constructor args...}} )

NOTIFY {{active sessions in GameSessionManager.$activeSessionCount}} // Should be 2

// End a session
$session1.EndSession() // Assuming EndSession is an instance method that calls the static logic
// Or, if EndSession was a STATIC method:
// GameSessionManager.EndSession(sessionId <- $session1.$sessionId) 

NOTIFY {{active sessions in GameSessionManager.$activeSessionCount}} // Should be 1

// Attempting to change a CONST static attribute would be an error:
// GameSessionManager.$MAX_SESSIONS <- 200 // ERROR! $MAX_SESSIONS is CONST
```

**In this example:**

- `$MAX_SESSIONS` is a `STATIC CONST` attribute. Its value (100) is set once and shared. It's used by the `CONSTRUCTOR` to check if a new session can be created.

- `$activeSessionCount` is a mutable `STATIC` attribute. It's initialized to 0 and is incremented by the `CONSTRUCTOR` of each new `GameSessionManager` object and decremented when a session ends. All parts of the program that refer to `GameSessionManager.$activeSessionCount` see the same, single, up-to-date count.
# Instance Attributes

Instance attributes hold the data that is unique to each **individual object (instance)** created from a class. Unlike class (static) attributes which are shared, every object gets its own separate set of instance attributes. This allows each object to maintain its own distinct state.

For example, if you have a `Player` class, each `Player` object would have its own `$playerName`, its own `$health`, and its own `$inventory`. Modifying the `$health` of one Player object does not affect the `$health` of another.

**Declaration Syntax:**

Instance attributes are declared directly within the `CLASS ... ENDCLASS` block, typically after any class (static) attributes. They are declared without the `STATIC` keyword. They can be mutable or constant (`CONST`).

1. **Mutable Instance Attribute:**
    
    ```
    [<visibility>] $camelCaseName AS DataType
    ```

2. **Constant Instance Attribute:**
    
    ```
    [<visibility>] CONST $UPPER_SNAKE_CASE_NAME AS DataType
    ```

- **`<visibility>`**: Optional [[Encapsulation|attribute visibility]] defaults to `PUBLIC`. Determines which client code can access the attribute.

- **No `STATIC` Keyword:** The absence of `STATIC` signifies that the attribute belongs to individual instances.

- **`CONST` Keyword (Optional):**
    
    - If `CONST` is present, the attribute's value can only be set once, which **must** happen within the class's `CONSTRUCTOR`. After the constructor finishes executing for an object, the value of its `CONST` instance attributes cannot be changed. The name (`$UPPER_SNAKE_CASE_NAME`) must be conformed to constants naming rules.
    
    - If `CONST` is absent, the attribute is mutable. Its value is typically initialized in the `CONSTRUCTOR` and can be changed later. The name (`$camelCaseName`) must be conformed to variables naming rules.

- **`AS DataType`**: Standard AlgoDraft syntax for annotation and typing.

- **No Direct Initialization at Declaration:** Instance attributes are **not** initialized with a value (`<- <value>`) directly where they are declared within the class block. Their initialization is the responsibility of the `CONSTRUCTOR`.

**Example:**

```
CLASS BankAccount
    // --- Instance Attributes ---

    // Public and Constant: Fixed information, accessible by anyone.
    PUBLIC CONST $ACCOUNT_NUMBER AS String    // Unique, unchangeable identifier
    PUBLIC CONST $ACCOUNT_HOLDER_NAME AS String // Name of the account holder

    // Public and Mutable: Information that can change and is accessible.
    PUBLIC $balance AS Real                   // Current account balance

    // Private and Mutable: Internal state, not directly accessible from outside.
    PRIVATE $transactionCount AS Integer      // Number of transactions made
    PRIVATE $isFrozen AS Boolean              // Internal flag if account is frozen

    // Private and Constant: Internal fixed property, set at creation.
    PRIVATE CONST $ACCOUNT_TYPE AS String     // E.g., "Savings", "Checking"

    // --- Constructor ---
    METHOD CONSTRUCTOR
        PARAMS
            $accNum AS String
            $holderName AS String
            $initialDeposit AS Real
            $accType AS String <- "Checking" // Default account type
        ENDPARAMS

        // Initialize CONST attributes (can only be done here)
        $this.$ACCOUNT_NUMBER <- $accNum
        $this.$ACCOUNT_HOLDER_NAME <- $holderName
        $this.$ACCOUNT_TYPE <- $accType

        // Initialize mutable attributes
        IF $initialDeposit >= 0.0 THEN
            $this.$balance <- $initialDeposit
        ELSE
            $this.$balance <- 0.0 // Ensure non-negative balance initially
            NOTIFY {{initial deposit cannot be negative. balance set to 0. as warning}}
        ENDIF
        $this.$transactionCount <- 0
        $this.$isFrozen <- FALSE
    ENDMETHOD

    // --- Public Methods (Behavior) ---
    PUBLIC METHOD Deposit(amount AS Real)
        PARAMS ENDPARAMS
        IF $this.$isFrozen == TRUE THEN
            NOTIFY "Account " & $this.$ACCOUNT_NUMBER & " is frozen. Deposit failed."
            RETURN
        ENDIF
        IF amount > 0.0 THEN
            $this.$balance <- $this.$balance + amount
            $this.$transactionCount <- $this.$transactionCount + 1
            NOTIFY "Deposited: " & amount & ". New balance: " & $this.$balance
        ELSE
            NOTIFY "Deposit amount must be positive."
        ENDIF
    ENDMETHOD

    PUBLIC METHOD Withdraw(amount AS Real)
        PARAMS ENDPARAMS
        IF $this.$isFrozen == TRUE THEN
            NOTIFY "Account " & $this.$ACCOUNT_NUMBER & " is frozen. Withdrawal failed."
            RETURN
        ENDIF
        IF amount > 0.0 THEN
            IF $this.$balance >= amount THEN
                $this.$balance <- $this.$balance - amount
                $this.$transactionCount <- $this.$transactionCount + 1
                NOTIFY "Withdrew: " & amount & ". New balance: " & $this.$balance
            ELSE
                NOTIFY "Insufficient funds for withdrawal."
            ENDIF
        ELSE
            NOTIFY "Withdrawal amount must be positive."
        ENDIF
    ENDMETHOD

    PUBLIC METHOD GetBalance RETURNS Real
        PARAMS ENDPARAMS
        RETURN $this.$balance
    ENDMETHOD

    PUBLIC METHOD GetAccountSummary RETURNS String
        PARAMS ENDPARAMS
        RETURN "Account: " & $this.$ACCOUNT_NUMBER & ", Holder: " & $this.$ACCOUNT_HOLDER_NAME & ", Balance: " & $this.$balance
    ENDMETHOD

    // --- Private Methods (Internal Helper) ---
    PRIVATE METHOD FreezeAccount
        PARAMS ENDPARAMS
        $this.$isFrozen <- TRUE
        NOTIFY "Account " & $this.$ACCOUNT_NUMBER & " has been frozen."
    ENDMETHOD

    PRIVATE METHOD UnfreezeAccount
        PARAMS ENDPARAMS
        $this.$isFrozen <- FALSE
        NOTIFY "Account " & $this.$ACCOUNT_NUMBER & " has been unfrozen."
    ENDMETHOD

ENDCLASS

// --- Using the BankAccount Class ---

$myAccount AS BankAccount <- NEW BankAccount(
                                accNum <- "ACC123456",
                                holderName <- "Alice Wonderland",
                                initialDeposit <- 500.00,
                                accType <- "Savings"
                            )

NOTIFY $myAccount.GetAccountSummary()
// Output: Account: ACC123456, Holder: Alice Wonderland, Balance: 500

$myAccount.Deposit(amount <- 150.00) // Balance becomes 650.00
$myAccount.Withdraw(amount <- 100.00) // Balance becomes 550.00

// Accessing public attributes:
NOTIFY "Current Balance: " & $myAccount.$balance // OK, $balance is PUBLIC
NOTIFY "Account Holder: " & $myAccount.$ACCOUNT_HOLDER_NAME // OK, $ACCOUNT_HOLDER_NAME is PUBLIC CONST

// Attempting to change CONST attributes would be an error:
// $myAccount.$ACCOUNT_NUMBER <- "NEWACC789" // ERROR! $ACCOUNT_NUMBER is CONST

// Attempting to access PRIVATE attributes from outside would be an error:
// NOTIFY "Transaction Count: " & $myAccount.$transactionCount // ERROR! $transactionCount is PRIVATE
// $myAccount.$isFrozen <- TRUE // ERROR! $isFrozen is PRIVATE (should use a method like FreezeAccount if exposed)

// The class can use its private methods internally
// $myAccount.FreezeAccount() // If this were called from a public method of BankAccount, it would be fine.
```

**Breakdown of Attribute Choices in `BankAccount`:**

- **`PUBLIC CONST $ACCOUNT_NUMBER AS String`**: Every account has a unique, unchangeable number visible to all.

- **`PUBLIC CONST $ACCOUNT_HOLDER_NAME AS String`**: The primary holder's name is fixed for this account instance and publicly visible.

- **`PUBLIC $balance AS Real`**: The balance can change and needs to be queryable (though modifications should ideally go through methods like `Deposit` and `Withdraw` to enforce rules).

- **`PRIVATE $transactionCount AS Integer`**: The number of transactions is internal data, managed by methods. It's mutable. Not directly relevant to the outside world, but important for internal tracking or fee calculation.

- **`PRIVATE $isFrozen AS Boolean`**: An internal state flag. Its modification should be controlled strictly by internal logic (e.g., via `FreezeAccount`/`UnfreezeAccount` methods, which themselves might be `PRIVATE` or `PUBLIC` depending on bank policy). It's mutable.

- **`PRIVATE CONST $ACCOUNT_TYPE AS String`**: The type of account (Savings, Checking) is set at creation and doesn't change. It might be used internally for different fee structures or interest calculations but isn't directly exposed as `PUBLIC`.
