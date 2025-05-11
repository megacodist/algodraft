---
status: Planned
---
The [Function Syntax in AlgoDraft | Google AI Studio](https://aistudio.google.com/prompts/1bYLDDA7tu6vZU6aO_cSAcdYB92s634_i) Gemini Chat Prompt is ready to produce tutorial with the following sections:

**X. Iteration: Processing Sequences and Collections**

**(Introduction to Iteration)**

- Briefly explain what iteration is...
    
- Why it's fundamental...
    
- Mention AlgoDraft's mechanisms...
    

**X.1. Basic Iteration: The FOR EACH Loop**

- (Pointer/summary of "FOR EACH in The flow of execution").
    
- Explain syntax and usage for iterating.
    
- Emphasize its role in consuming iterable sequences.
    

**X.2. The Foundation of Iteration: Iterable Data Structures and Interfaces**

- Introduce the core concepts that make FOR EACH work.
    
- **X.2.1. Iterable Data Structures**
    
    - (This is where your "Iterability in Data Structures" content goes).
        
    - Explain that many built-in data structures in AlgoDraft (e.g., List, Array, String, Set, Map keys/values/entries) are inherently iterable.
        
    - Describe what makes them iterable – usually by them implicitly or explicitly conforming to the **IIterable** interface (see next point).
        
    - Provide examples of using FOR EACH directly with these common data structures.
        
    - This section answers the question: "What things can I directly loop over with FOR EACH?"
        
- **X.2.2. The IIterable Interface**
    
    - (Pointer/summary of your "IIterable Interface in Interfaces" section).
        
    - Formally define the **IIterable** interface (e.g., it mandates a method like GetIterator() -> Iterator).
        
    - Explain that any custom data type implementing this interface also becomes compatible with FOR EACH and other iteration mechanisms.
        
    - Connect this back to X.2.1 by stating that built-in iterable data structures implement this interface (or an equivalent internal mechanism).
        
- **X.2.3. The Iterator Basic Type**
    
    - (Pointer/summary of "Iterator Basic Type in Basic Data Types").
        
    - Explain the Iterator's role: providing sequential access (MoveNext(), CurrentItem()).
        
    - Briefly explain how **IIterable** objects produce Iterators, and how FOR EACH might use an Iterator internally.
        

**X.3. Creating Custom Iterable Sequences: Streams (Generators)**

- Introduce Streams/Generators as a convenient way to define your own **IIterable** sequences, often without explicitly creating a class that implements **IIterable** and Iterator manually.
    
- Explain STREAM FUNCTION (or similar) syntax and the YIELD statement.
    
- Benefits: lazy evaluation, memory efficiency, etc.
    
- Examples of creating and consuming streams.