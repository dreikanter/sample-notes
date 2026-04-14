# Crafting Interpreters – parsing notes

Continuing from [[20260123_2046]]. Chapters 4–6 cover representing and parsing code.

## Abstract syntax trees

After scanning (tokens) comes parsing (trees). The AST represents the grammatical structure of the code. Each node is an expression or statement type.

Nystrom uses a visitor pattern (with some self-consciousness about it) to process AST nodes without putting all processing logic into the node classes. The trade-off: adding new operations is easy, adding new node types requires touching all visitors.

```java
interface ExprVisitor<R> {
    R visitBinaryExpr(Expr.Binary expr);
    R visitUnaryExpr(Expr.Unary expr);
    R visitLiteralExpr(Expr.Literal expr);
    // etc.
}
```

## Recursive descent parsing

The parser works top-down, starting from the highest-level grammar rule and recursing down. Each grammar rule becomes a method.

Lox grammar (simplified):
```
expression → equality
equality   → comparison (("==" | "!=") comparison)*
comparison → term ((">" | "<" | ">=" | "<=") term)*
term       → factor (("+" | "-") factor)*
factor     → unary (("*" | "/") unary)*
unary      → ("!" | "-") unary | primary
primary    → NUMBER | STRING | "true" | "false" | "nil" | "(" expression ")"
```

The precedence hierarchy is encoded in the grammar: higher-precedence operators appear in rules closer to `primary`. This is elegant — the grammar structure *is* the precedence table.

## Error recovery

When parsing hits a bad token, it throws a `ParseError`. But the parser *recovers* using "synchronize" — discards tokens until it finds one that can start a new statement (like a semicolon or keyword). This lets multiple errors be reported instead of stopping at the first.

Good error reporting is underappreciated work.

Source: https://craftinginterpreters.com/parsing-expressions.html
