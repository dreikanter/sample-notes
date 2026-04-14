# Patterns I keep relearning in software

Not design patterns in the Gang of Four sense — more like recurring observations about what makes code good or bad in practice, accumulated from the past few years.

**On abstractions**: abstractions that exist to serve DRY rather than to model something real tend to become liabilities over time. A function called `handleThing` that does slightly different things in different contexts is worse than two functions with honest names. [Sandy Metz's talk on the wrong abstractions](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) nails this.

**On readability**: code is read far more than it's written. Every naming and structural decision is a communication decision, not just a technical one. The test: can someone who doesn't know the context follow this code without running it?

**On error handling**: the place where most codebases quietly accumulate debt. Error cases are less interesting to write and test than happy paths. They're usually where the actual complexity lives. A system that handles errors badly is fragile at its seams.

**On the complexity budget**: every component has an implicit complexity budget. When you spend it on cleverness, you have less left for the problems that actually need it. The boring solution is often the best solution.

**On tests**: tests that test implementation rather than behaviour break constantly and give false confidence. The question isn't "does this method do X" but "does this system behave correctly when...".

**On performance**: measure first. Almost every premature optimisation I've encountered in the wild was optimising something that wasn't the bottleneck.

**On documentation**: the best documentation is code that needs less documentation. After that: precise comments on the *why*, never the *what*.
