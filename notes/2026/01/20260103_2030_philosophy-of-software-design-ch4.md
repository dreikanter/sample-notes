# Philosophy of software design – chapter 4 notes

Chapter 4: "Modules Should Be Deep"

Ousterhout's central metaphor: think of a module as a rectangle. The top edge is the interface (what callers see). The area is the functionality (what it does). A "deep" module has a small top edge relative to its area — narrow interface, rich functionality. A "shallow" module has a wide interface relative to its area — lots of methods, each doing a little.

Unix file I/O is the canonical deep module example: `open`, `read`, `write`, `close`. Five calls, enormous underlying complexity managed invisibly.

Java's `LinkedList` is his shallow module example: dozens of public methods for a data structure that most callers use in two or three ways. The interface complexity leaks cognitive load onto callers without providing proportional value.

**Why this matters**: every interface element is a dependency. Every parameter a caller must understand is complexity the module has not hidden. The test is: does this interface element need to exist, or is it an implementation detail that leaked out?

**Where I think he understates nuance**: there are cases where explicit interfaces are valuable for testing, type safety, or forcing callers to think. A function that takes ten parameters with clear names can be clearer than one that takes an options object. He waves at this in a footnote but doesn't give it much weight.

The chapter connects to the ideas in the "Information Hiding" concept from Parnas (1972): https://dl.acm.org/doi/10.1145/361598.361623

This was noted as a book-level reference in [[20251229_2020]].
