---
status: Planned
---
this is the syntax of operator overloading in AlgoDraft

OPERATOR NOT $this RETURNS ReturnType    
    // ... logic for '+' ...
ENDOPERATOR

OPERATOR $this++ RETURNS ReturnType    
    // ... logic for '+' ...
ENDOPERATOR

OPERATOR $this + $other RETURNS ReturnType
    OPERANDS
        $other AS DataType
    ENDOPERANDS
    
    // ... logic for '+' ...
ENDOPERATOR

Operator overloading is actually an instance method because we use $this in their definition.
They are only PUBLIC because they're going to be used as syntax suagr.
