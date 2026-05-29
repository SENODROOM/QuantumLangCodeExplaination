# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an integral part of the Quantum Language compiler's parser, designed to interpret expressions involving arithmetic operations such as multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). It ensures that these operations are parsed correctly according to the rules of the language's grammar.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: `std::unique_ptr<ASTNode>` - A pointer to an abstract syntax tree node representing the parsed expression.

## How It Works

The function begins by calling `parsePower()`, which parses the left-hand side of the expression. It then enters a loop that continues as long as the next token is one of the specified arithmetic operators (`*`, `/`, `%`, or `//`). Inside the loop:

1. The line number of the current token is recorded.
2. The operator token is consumed and its value is stored in the variable `op`.
3. Another call to `parsePower()` is made to parse the right-hand side of the expression.
4. An `ASTNode` containing a `BinaryExpr` is created, encapsulating the operator and the two operands. The new node replaces the previous left-hand side node.
5. The process repeats until there are no more arithmetic operators following the current expression.

Finally, the function returns the root of the parsed expression tree.

## Edge Cases

1. **Empty Expression**: If the input stream is empty or contains only non-arithmetic tokens, the function will return the result of `parsePower()`, which should handle such cases gracefully.
2. **Nested Expressions**: The function can handle nested expressions involving multiplication, division, etc., due to the recursive nature of `parsePower()`.
3. **Invalid Operators**: If invalid arithmetic operators are encountered, the function will continue parsing up to the next valid token or end of the expression.

## Interactions with Other Components

- **Grammar Rules**: `parseMulDiv` relies on the grammar rules defined in the Quantum Language specification to determine when to stop parsing.
- **Error Handling**: Errors related to unexpected tokens or malformed expressions are handled by the parser's error reporting mechanism.
- **AST Construction**: `parseMulDiv` constructs an abstract syntax tree (AST) that represents the parsed expression, facilitating further semantic analysis and code generation stages of the compilation process.