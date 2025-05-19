---
status: Writing
---
In algorithm design, it's often necessary to determine the specific type of a piece of data or to check if an object possesses certain capabilities before performing operations on it. This is crucial for:

- Ensuring that an operation is valid and safe for a given object.

- Handling items in [[Homogeneity|heterogeneous]] collections in a customized way.

- Implementing conditional logic that branches based on the nature or structure of an object.

- Making assertions or verifying preconditions about the data your algorithm works with.

AlgoDraft provides two distinct **Boolean binary operators** to facilitate these checks. They are "Boolean" because they always yield a Boolean result (`TRUE` or `FALSE`), and "binary" because they operate on exactly two operands: an object on the left and a type or interface identifier on the right. These operators are:

- **`IS A/IS AN`**: For checking **nominal type identity**. This verifies if an object belongs to a specific named type through its declaration, inheritance, or explicit interface implementation.

- **`CONFORMS TO`**: For checking **structural type compatibility** (often known as "duck typing"). This verifies if an object possesses the required attributes (fields) and methods defined by a specific interface, regardless of its declared type.

Let's explore each operator in detail.

# Nominal Type Checking: The `IS A / IS AN` Boolean Binary Operator

The `IS A/IS AN` operator is used to determine if a given object is an instance of a specific, named `CLASS` or `RECORD`, if it belongs to a known inheritance hierarchy, or if it explicitly `IMPLEMENTS` a specific `INTERFACE`. This operator checks the "named" or "declared" type relationship.

**Syntax:**

```
object_operand IS A TypeNameOrInterfaceName
object_operand IS AN TypeNameOrInterfaceName
```

- `object_operand`: An identifier that resolves to an object instance (e.g., a variable holding an object).

- `TypeNameOrInterfaceName`: An identifier representing a defined `CLASS`, `RECORD`, or `INTERFACE` in AlgoDraft (these could be defined directly or within a `CONCEPT` block).

- The choice between `A` and `AN` follows standard English grammar, depending on whether `TypeNameOrInterfaceName` begins with a consonant sound (use `A`) or a vowel sound (use `AN`). For example:
    
    - `$myObject IS A Button`
    
    - `$myObject IS AN Image`
    
    - `$myObject IS AN IUserInterface`

**Semantics:**

The expression `object_operand IS A/AN TypeNameOrInterfaceName` evaluates to a Boolean result (`TRUE` or `FALSE`).

- It returns `TRUE` if the runtime type of the `object_operand` satisfies one of the following conditions with respect to `TypeNameOrInterfaceName`:
    
    1. The object is an instance of the exact `CLASS` or `RECORD` specified by `TypeNameOrInterfaceName`.
    
    2. The object is an instance of a `CLASS` that `INHERITS` (directly or indirectly) from the `CLASS` specified by `TypeNameOrInterfaceName`.
    
    3. The object is an instance of a `CLASS` or `RECORD` that explicitly `IMPLEMENTS` the `INTERFACE` specified by `TypeNameOrInterfaceName` (via `CLASS ... IMPLEMENTS IMyInterface ...`  or `RECORD ... IMPLEMENTS IMyInterface ...`).
        
- It returns `FALSE` in all other cases.

**Use Cases:**

- Verifying an object's specific declared type or its lineage within a formal inheritance hierarchy.

- Confirming that an object explicitly adheres to a contract declared via an `IMPLEMENTS` clause.

- Safely downcasting or performing type-specific operations after confirming the type.

**Example:**

```
CONCEPT
    INTERFACE IClickable := {{an interface for UI elements that support a click action}} ENDINTERFACE
    CLASS UIElement := {{the base class for all graphical UI elements}} ENDCLASS
    CLASS Button INHERITS UIElement IMPLEMENTS IClickable := {{a UI button that is a UIElement and is IClickable}} ENDCLASS
    CLASS SpecialButton INHERITS Button := {{a specialized button, inheriting Button's traits}} ENDCLASS
    CLASS Icon IMPLEMENTS IClickable := {{a data record for an icon, which is also conceptually clickable}} ENDCLASS
ENDCONCEPT

$btn AS Button <- NEW Button()
$specBtn AS SpecialButton <- NEW SpecialButton()
$ico AS Icon <- NEW Icon()
$element AS ANY // 'ANY' can hold any data type

$element <- $btn
IF $element IS A Button THEN NOTIFY {{$element is a Button}}                 // TRUE
IF $element IS A UIElement THEN NOTIFY {{$element is a UIElement by inheritance}} // TRUE
IF $element IS AN IClickable THEN NOTIFY {{$element is an IClickable}}      // TRUE (Button implements IClickable)

$element <- $specBtn
IF $element IS A SpecialButton THEN NOTIFY {{$element is a SpecialButton}}  // TRUE
IF $element IS A Button THEN NOTIFY {{$element is a Button by inheritance}}    // TRUE
IF $element IS AN IClickable THEN NOTIFY {{$element is an IClickable}}      // TRUE (inherits IClickable from Button)

$element <- $ico
IF $element IS A Icon THEN NOTIFY {{$element is an Icon record}}             // TRUE
IF $element IS AN IClickable THEN NOTIFY {{$element is an IClickable}}      // TRUE (Icon implements IClickable)
IF $element IS A Button THEN NOTIFY {{$element is a Button}}                 // FALSE
```

# Structural Type Checking (Duck Typing): The CONFORMS TO Boolean Binary Operator

The `CONFORMS TO` operator is used to determine if a given object, regardless of its declared type, inheritance, or explicit `IMPLEMENTS` clauses, possesses a specific set of structural characteristics. In AlgoDraft, this primarily means checking for the presence of required **attributes (fields)** and **methods** as defined by an AlgoDraft `INTERFACE`. This embodies the principle of "duck typing":

> If something (an object) looks like a duck (has the expected attributes) and acts like a duck (has the expected methods with compatible signatures), so it can be treated as a duck (a good fit for some scenarios).

**Syntax:**

```
object_operand CONFORMS TO InterfaceName
```

- `object_operand`: An identifier that resolves to an object instance.

- `InterfaceName`: An identifier that must refer to an `INTERFACE` defined in AlgoDraft (either directly or within a `CONCEPT` block).

**Semantics:**

The expression `object_operand CONFORMS TO InterfaceName` evaluates to a Boolean result (`TRUE` or `FALSE`).

- It returns `TRUE` if the `object_operand` structurally provides **all** the public attributes (fields) with matching names and compatible types, AND **all** the public methods with matching names and compatible signatures (parameter count, directionality, types, and return type) as defined in the `InterfaceName`.

- The `object_operand`'s type does **not** need to have an explicit `IMPLEMENTS InterfaceName` clause for this check to return `TRUE`. The check is purely structural.

- It returns `FALSE` if there is at least one attribute or method specified in the `InterfaceName` is missing or has an incompatible signature in the `object_operand`.

**Use Cases:**

- Interacting flexibly with objects that share common structural characteristics (both data fields and methods), even if they originate from different class hierarchies or don't formally implement a common named interface.

- Writing algorithms that can adapt to different object types, provided they offer the necessary "shape" (attributes and methods) defined by an `INTERFACE`.
    
- Enabling a form of ad-hoc polymorphism based on structural similarity.

**Example:**

```
// Interface defining required state (attributes) and behavior (methods)
INTERFACE ILoggableEntity :=
    // Expected Attributes
    $entityID AS String,
    $logLevel AS Integer,
    // Expected Behavior
    METHOD GenerateLogEntry(IN $message AS String) -> String,
    METHOD SetLogLevel(IN $newLevel AS Integer),
ENDINTERFACE

CLASS AuditService := // Does NOT explicitly implement ILoggableEntity
    $serviceName AS String,
    $entityID AS String,       // Matches ILoggableEntity attribute
    $logLevel AS Integer,      // Matches ILoggableEntity attribute
    $isActive AS Boolean,

	METHOD CONSTRUCTOR(
			$serviceName AS String,
		    $entityID AS String,
		    $logLevel AS Integer <- 2,
		    $isActive AS Boolean <- TRUE,
		    ) :=
		$this.$serviceName <- $serviceName
		$this.$entityID <- $entityID
		$this.$logLevel <- $logLevel
		$this.$isActive <- $isActive$
	ENDMETHOD

    METHOD GenerateLogEntry(IN $eventMessage AS String) -> String := // Parameter name different, type matches
        RETURN $serviceName + " (ID: " + $entityID + ") Log [" + $logLevel + "]: " + $eventMessage
    ENDMETHOD

    METHOD SetLogLevel(IN $level AS Integer) :=
        $this.$logLevel <- $level
        NOTIFY {{log level for $serviceName set to $level}}
    ENDMETHOD

    METHOD StartService() := NOTIFY {{$serviceName started}} ENDMETHOD
ENDCLASS

CLASS SimpleTask :=
    $taskName AS String,
    $entityID AS String,       // Matches attribute
    
	METHOD CONSTRUCTOR(
			$taskName AS String,
		    $entityID AS String,
		    ) :=
		$this.$taskName <- $taskName
		$this.$entityID <- $entityID
	ENDMETHOD
	
    // Missing $logLevel attribute
    METHOD GenerateLogEntry(IN $detail AS String) -> String :=
        RETURN "Task '" + $taskName + "' Note: " + $detail
    ENDMETHOD
    // Missing SetLogLevel method
ENDCLASS

$audit AS AuditService <- NEW AuditService("MainAuditor", "AUD001", 2)
$task AS SimpleTask <- NEW SimpleTask("DataProcessing", "TSK007")

IF $audit CONFORMS TO ILoggableEntity THEN
    NOTIFY {{$audit.GenerateLogEntry("System check passed")}} // Assuming direct call after successful check
    $audit.SetLogLevel(1)
    NOTIFY {{$audit structurally conforms to ILoggableEntity}}
ENDIF
// Output for $audit would indicate conformity and log messages.

IF $task CONFORMS TO ILoggableEntity THEN
    NOTIFY {{$task structurally conforms to ILoggableEntity}}
ELSE
    NOTIFY {{$task does not structurally conform to ILoggableEntity, likely missing $logLevel attribute or SetLogLevel method}}
ENDIF
// Output for $task would indicate non-conformity.
```

# Comparing `IS A/AN` and `CONFORMS TO`

These two Boolean binary operators serve distinct but complementary roles in type checking:

- **`object_operand IS A/AN TypeNameOrInterfaceName`**:
    
    - Checks **nominal type relationships**.
    
    - Focuses on the declared name of the type, its formal inheritance hierarchy, or an explicit `IMPLEMENTS` clause.
    
    - Asks: "Is this object formally declared or known to be of this kind, or does it explicitly promise this contract by name?"

- **`object_operand CONFORMS TO InterfaceName`**:
    
    - Checks **structural type compatibility**.
    
    - Focuses on the actual presence and signature compatibility of attributes and methods as defined by the `InterfaceName`, irrespective of formal declarations or inheritance.
    
    - Asks: "Does this object structurally possess the required fields and methods as specified by this interface, regardless of its declared type name?"

**When to Use Which:**

- Use **`IS A/AN`** when the specific, formally declared type identity of an object, or its explicit adherence to a named contract through inheritance or `IMPLEMENTS`, is critical to your algorithm's logic.

- Use **`CONFORMS TO`** when your algorithm needs to ensure an object can provide certain data (attributes) and perform certain actions (methods) as defined by an interface, promoting greater flexibility and decoupling from strict type hierarchies.