---
status: Draft
---
Iteration is one of the most fundamental concepts in programming and algorithm design. At its heart, iteration is the process of **sequentially accessing and processing each element within a collection or a generated sequence, one item at a time.** Instead of dealing with an entire set of data all at once, iteration allows you to focus on individual elements in a defined order.

Think of everyday examples:

- Reading a book, processing it page by page.

- Dealing cards one by one from a deck.

- Working through a list of tasks, completing each one before moving to the next.

This one-by-one processing is a foundational technique used in countless algorithms to examine, transform, or utilize data.

# Why Iteration?

Iteration is a cornerstone of effective algorithm design in AlgoDraft for several key reasons:

- **Fundamental to Algorithms:** Many algorithms are inherently iterative. For instance, searching for a specific item in a dataset involves examining items one by one until a match is found. Calculating the sum of numbers in a list requires adding each number sequentially.

- **Processing Collections:** Iteration is the primary method for working with the contents of various data structures provided by AlgoDraft, such as Lists, Tuples (for accessing their elements in order), Strings (for processing characters), Sets etc.

- **Enabling Lazy Evaluation and Efficiency:** While iteration itself is about sequential access, it is a key mechanism that **paves the way for lazy evaluation**. Lazy evaluation means that elements of a sequence can be generated or fetched only when they are actually needed by the iteration process. This strategy, often employed with the underlying machinery of iteration (called iterators), is crucial for:
    
    - **Memory Efficiency:** Handling very large datasets that might not fit entirely into memory at once. Only the current item (or a small buffer) needs to be active.
    
    - **Infinite Sequences:** Working with potentially infinite sequences, such as all natural numbers, a continuous stream of sensor data, or prime numbers generated on demand.
    
    - **Performance:** Avoiding unnecessary computation. If an algorithm only needs the first few items that satisfy a condition from a long potential sequence, lazy evaluation ensures that only those necessary items are computed or fetched.

- **Abstraction:** Iteration provides a uniform way to access elements. When you iterate over a collection, your logic focuses on processing each item, abstracting you away from the specific details of how that collection stores or generates its elements internally.

# Key Components of Iteration in AlgoDraft (The "Big Picture" Teaser)

AlgoDraft provides a cohesive set of concepts and tools to support powerful and flexible iteration. These components work together:

- **Iterators (`Iterator` basic type):** These are the fundamental "engines" or, to use an analogy, "vending machines" of iteration. An iterator is an object that knows how to step through a sequence, keeping track of the current position, and can yield (or "vend") elements one by one.

- **Iterables (`IIterable` interface):** This is a contract that defines objects which can produce an iterator. Collections like Lists and Tuples are "iterable" because they can provide an iterator to walk through their contents. Implementing `IIterable` means an object can be subjected to iteration, among its other potential responsibilities.

- **Built-in Integer Iterables:** Iterating over ranges of integers is extremely common in algorithms (e.g., for counted loops, accessing array/list indices). Because of this, AlgoDraft provides dedicated, built-in types and convenient literal syntaxes for defining these common integer streams. These types themselves are iterable.

- **Iteration Functions:** Beyond built-in integer iterables, AlgoDraft offers a versatile mechanism for designers to create their own **custom iterators** using special functions. These "Iteration Functions" can lazily `YIELD` streams of any data type, providing a powerful way to define custom data generation or traversal logic.

- **Iteration Flow of Execution**: The primary high-level construct in AlgoDraft for performing iteration is the **`FOR EACH ... IN ... DO ... ENDFOR` loop**. This loop is designed to work seamlessly with any data source that is either an `IIterable` object (from which it internally obtains a fresh iterator) or an Iterator object directly.

# Roadmap for Iteration

By exploring this "Iteration" section, you will gain a comprehensive understanding of:

- The role and mechanics of **Iterators** as the core engine for stepping through sequences.

- **Iterable objects** and the `IIterable` interface, which defines how collections and other data sources provide iterators.

- AlgoDraft's specialized built-in types for conveniently generating common streams of integers.

- How to create your own custom, lazy streams of data for virtually any purpose using **Iteration Functions** and the `YIELD` keyword.

Mastering these concepts will unlock the ability to design efficient, memory-friendly, and highly expressive algorithms in AlgoDraft that can process sequences of data, large or small, finite or potentially infinite.