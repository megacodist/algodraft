---
status: Draft
---
In algorithm design with AlgoDraft, your logic will often need to interact with systems, libraries, data, or functionalities that are "external" to the core algorithm itself. These can include operating system features (like file access or environment variables), database interactions, calls to external APIs, use of pre-existing complex data structures, or hardware interfaces.

To manage these interactions clearly and effectively at the design stage, AlgoDraft provides two primary mechanisms:

1. **`CONCEPT ... ENDCONCEPT` blocks:** Used for declaring abstract, conceptual external entities. This approach is ideal for high-level design where you want to focus on what an external entity represents or does, described using Natural Language Descriptions (NLDs), without tying your design to specific implementations.

2. **`IMPORT ... ENDIMPORT` blocks:** Used for referencing more concrete external entities that are understood to come from specified external sources (like a conceptual library or module). This bridges your design closer to potential implementation details by acknowledging a more defined origin for these entities.

This section will detail both `CONCEPT` and `IMPORT` blocks, explaining their syntax, purpose, and how to use them to define external types, constants, and functions, allowing your AlgoDraft algorithms to remain focused yet aware of their external dependencies.

