# Reading Crafting Interpreters – chapters 2–3

Continuing from [[20260105_2032]].

Chapter 2 defines the Lox language: dynamically typed, garbage collected, supports closures. Small enough to implement in a book, large enough to be interesting. The design choices are explained, which is unusual for programming books.

Chapter 3 is the scanner. This is the first real code.

The scanner converts a source string into a flat list of tokens. Each token has a type (NUMBER, STRING, IDENTIFIER, various keywords and symbols), the lexeme (the actual source text), and a line number for error reporting.

**Key insight from the implementation**: the scanner uses a cursor approach. `start` marks the beginning of the current token; `current` advances through the string. When you've consumed enough characters to identify a token, you emit it from `start` to `current`.

Two-character tokens (like `!=`, `==`, `<=`) require looking ahead one character. The `match()` method handles this — advance only if the next character matches.

**String literals**: Nystrom's scanner handles them by consuming characters until it hits a closing `"`, with multi-line support. Error case: EOF before closing quote. The scanner reports the error but continues — this is important. A good scanner recovers and keeps going so you can report multiple errors, not just the first one.

**Number literals**: Consume digits, then check for `.` followed by more digits (floating point). Lox has only doubles, no integer type.

The code is clean Java, about 150 lines so far. Seeing a real lexer of this size makes it less mysterious than the concept suggested.

Next: Chapter 4, representing code as trees.

Source: https://craftinginterpreters.com/scanning.html
