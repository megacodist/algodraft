---
status: Draft
---
Encapsulation is one of the fundamental principles of **Object-Oriented Programming (OOP)**. At its core, it means two things:

1. **Bundling:** Grouping related data (attributes) and the methods (functions) that operate on that data together within a single unit – the **Class**. We've already seen this when defining classes with attributes and methods.

2. **Information Hiding:** Restricting direct access to some of an object's internal components, primarily its data. Instead of allowing external code to freely modify an object's attributes, access is typically controlled through the object's public methods.

# Achieving Encapsulation: Visibility Modifiers

In AlgoDraft, the primary mechanism for achieving information hiding and controlling access to class members (attributes and methods) is through **visibility modifiers**. These keywords specify the level of access allowed to a class member from different parts of your program.

AlgoDraft supports three visibility keywords for class members:

1. **`PUBLIC`**

2. **`PRIVATE`**

3. **`PROTECTED`**

If no visibility keyword is specified for an attribute or method, its visibility **defaults to `PUBLIC`** in AlgoDraft.

**Syntax:**

The visibility keyword precedes the declaration of an attribute or method:

```
CLASS MyExampleClass

    // Attributes
    PUBLIC $publicAttribute AS String
    PRIVATE $secretData AS Integer
    PROTECTED $internalValue AS Boolean
    $defaultVisibilityAttribute AS Real // Defaults to PUBLIC

    // Methods
    PUBLIC METHOD DoSomethingPublicly
        PARAMS ENDPARAMS
        // ...
    ENDMETHOD

    PRIVATE METHOD PerformInternalCalculation
        PARAMS ENDPARAMS
        // ...
    ENDMETHOD

    PROTECTED METHOD HelperForSubclasses
        PARAMS ENDPARAMS
        // ...
    ENDMETHOD

    METHOD DefaultVisibilityMethod // Defaults to PUBLIC
        PARAMS ENDPARAMS
        // ...
    ENDMETHOD

ENDCLASS
```

Let's explore each visibility level and how it contributes to encapsulation:

## `PUBLIC` Members

- **Accessibility:** `PUBLIC` attributes and methods can be accessed from **anywhere**:
    
    - From within other methods of the same class.
    
    - From methods of subclasses.
    
    - From outside the class, by any code that has an instance of the class (for instance members) or by using the class name (for static members).

- **Role in Encapsulation:** `PUBLIC` members form the **external interface** of your class. These are the "controls" you expose to the outside world. A well-designed class carefully chooses what to make public, exposing only what's necessary for interaction.

- **Default:** Remember, if you don't specify a visibility, it defaults to `PUBLIC`.

**Example:**

```
CLASS SimpleCounter
    // It's often better practice to make attributes PRIVATE and provide PUBLIC methods (getters/setters)
    // But for a simple PUBLIC example:
    PUBLIC $value AS Integer
	
    METHOD CONSTRUCTOR
        $this.$value <- 0
    ENDMETHOD
	
    PUBLIC METHOD Increment
        $this.$value <- $this.$value + 1
    ENDMETHOD
ENDCLASS

$counter AS SimpleCounter <- NEW SimpleCounter()
$counter.Increment()       // Accessing PUBLIC method
$counter.$value <- 10      // Directly accessing PUBLIC attribute
```

## `PRIVATE` Members

- **Accessibility:** `PRIVATE` attributes and methods can **only** be accessed from **within other methods of the same class** where they are defined.
    
    - They cannot be accessed directly from outside the class.
    
    - They cannot be accessed by subclasses.

- **Role in Encapsulation:** `PRIVATE` members are the cornerstone of information hiding. They protect the internal state and implementation details of a class. This allows the internal workings to change without breaking external code, as long as the `PUBLIC` interface remains consistent.

**Example:**

```
CLASS SecureDataStore
    PRIVATE $dataKey AS String
    PRIVATE $secretValue AS String

    METHOD CONSTRUCTOR
        PARAMS
	        $key AS String
	        $initialValue AS String
        ENDPARAMS
        
        $this.$dataKey <- $this.GenerateSecureKey($baseKey <- $key) // Call to a PRIVATE method
        $this.$secretValue <- $initialValue
    ENDMETHOD

    PUBLIC METHOD UpdateValue
        PARAMS
	        $keyAttempt AS String
	        $newValue AS String
        ENDPARAMS
        
        IF $this.IsKeyValid($keyToCheck <- $keyAttempt) THEN // Call to a PRIVATE method
            $this.$secretValue <- $newValue // Modifying PRIVATE attribute
            NOTIFY {{value updated successfully}}
        ELSE
            NOTIFY {{invalid key, update failed as error}}
        ENDIF
    ENDMETHOD

    PUBLIC METHOD GetValue RETURNS String OR NULL
        PARAMS
	        $keyAttempt AS String
        ENDPARAMS
        
        IF $this.IsKeyValid($keyAttempt) THEN
            RETURN $this.$secretValue // Accessing PRIVATE attribute
        ELSE
            NOTIFY {{invalid key, access denied as error}}
            RETURN NULL // Or handle error appropriately
        ENDIF
    ENDMETHOD

    PRIVATE METHOD GenerateSecureKey RETURNS String // PRIVATE helper
        PARAMS
	        $baseKey AS String
        ENDPARAMS
        // ... complex key generation logic ...
        RETURN DO {{generate a secure hash for $baseKey}}
    ENDMETHOD

    PRIVATE METHOD IsKeyValid RETURNS Boolean // PRIVATE helper
        PARAMS
	        $keyToCheck AS String
        ENDPARAMS
        
        RETURN $keyToCheck = $this.$dataKey
    ENDMETHOD
ENDCLASS

$store AS SecureDataStore <- NEW SecureDataStore("myApp", "Initial data")
$store.UpdateValue("SECURE_myApp", "Updated data") // OK

// The following would be ERRORS because $dataKey, $secretValue, GenerateSecureKey, IsKeyValid are PRIVATE:
// $store.$secretValue <- "Hacked!"
// $isValid AS Boolean <- $store.IsKeyValid("SECURE_myApp")
```

In this `SecureDataStore`, the `$dataKey` and `$secretValue` are kept private. All interactions happen through `PUBLIC` methods like `UpdateValue` and `GetValue`, which internally use private helper methods and access private data, ensuring data integrity and hiding the key validation logic.
## `PROTECTED` Members

- **Accessibility:** `PROTECTED` attributes and methods can be accessed:
    
    - From within other methods of the same class where they are defined (just like `PRIVATE`).
    
    - From within methods of **subclasses** (derived classes) of this class.
    
    - They generally cannot be accessed directly from outside the class or by unrelated classes.

- **Role in Encapsulation:** `PROTECTED` offers a way to share implementation details or state with closely related classes (subclasses) while still hiding them from the general public. It's useful for designing extensible class hierarchies.

**Example:**

```
CLASS BaseWidget
    PROTECTED $widgetId AS String
    PROTECTED $isEnabled AS Boolean

    METHOD CONSTRUCTOR
        PARAMS
	        $id AS String
        ENDPARAMS
        
        $this.$widgetId <- $id
        $this.$isEnabled <- TRUE
    ENDMETHOD

    PROTECTED METHOD RenderCore // Core rendering logic, shared by subclasses
        NOTIFY {{core rendering of $this.$widgetId is being performed}}
    ENDMETHOD

    PUBLIC METHOD Disable
        $this.$isEnabled <- FALSE
    ENDMETHOD
ENDCLASS

// Imagine: CLASS ButtonWidget EXTENDS BaseWidget
// Inside ButtonWidget, methods could access $this.$widgetId and call $this.RenderCore()
// because they are PROTECTED in BaseWidget.
```

# Benefits of Encapsulation

- **Data Integrity:** By making attributes `PRIVATE` and controlling access through `PUBLIC` methods, you can enforce validation rules and ensure the object's state remains consistent and valid.

- **Reduced Complexity (for the user of the class):** Users only need to understand the `PUBLIC` interface. They don't need to worry about the internal details, making the class easier to use correctly.

- **Increased Maintainability and Flexibility:** The internal implementation of a class (its `PRIVATE` members) can be changed or refactored without breaking external code, as long as the `PUBLIC` interface remains unchanged. This allows for easier updates and bug fixes.

- **Improved Modularity:** Well-encapsulated classes are like self-contained modules that can be developed, tested, and reused more independently.

By thoughtfully applying visibility modifiers, you leverage encapsulation to create classes in AlgoDraft that are more robust, secure, and easier to manage as your algorithms grow in complexity.