# Reading Crafting Interpreters – chapter 1

Starting the Nystrom book (https://craftinginterpreters.com). Free online, which is unusually generous for a book this good.

Chapter 1 is orientation. Nystrom lays out what a language implementation is and why you'd want to understand one: not because you'll build production languages, but because understanding what your tools do makes you better at using them — and occasionally, you need to build a small DSL or parser yourself.

The map of the territory:

1. **Scanning** (lexing): raw text → tokens
2. **Parsing**: tokens → AST (abstract syntax tree)
3. **Static analysis**: binding, type-checking, scope resolution
4. **Intermediate representations**: optional transformations
5. **Optimization**: also often optional
6. **Code generation**: AST → bytecode or machine code
7. **Runtime**: execution

The book will build two complete implementations of the "Lox" language: one tree-walk interpreter in Java (jlox), one bytecode VM in C (clox). The two-implementation structure is clever — you learn the same concepts twice, the second time more efficiently.

First observation from chapter 1: the reason lexers are usually a separate phase (rather than doing it all in one pass) is that context-free grammars can't express all of what most languages need. The string `/*` means something different depending on whether you're inside a string literal or in code. You need a stateful scanner to handle that, and it's cleaner to separate it.

Looking forward to the actual implementation. The code examples are clean Java.

Will track progress in separate notes as I go.
