# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an essential component of the Quantum Language compiler's parser, responsible for interpreting expressions that involve multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures proper parsing and evaluation of these binary operations in the quantum language syntax tree.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed expression. The node contains a `BinaryExpr` object which holds the operation type, the left operand, and the right operand.

## How It Works

1. **Initial Parsing**: The function starts by calling `parsePower()`, which parses expressions involving unary operators like negation (`-`) or exponentiation (`**`). The result of this call is stored in the variable `left`.

2. **Loop for Operators**: The function then enters a loop that continues as long as the next token is one of the specified binary operators (`*`, `/`, `%`, `//`). Inside the loop:
   - It records the line number where the operator occurs using `current().line`.
   - It consumes the current token, extracting its value (the operator symbol).
   - It recursively calls `parsePower()` again to parse the right-hand side of the expression, storing the result in `right`.
   - It creates a new `ASTNode` containing a `BinaryExpr` object initialized with the operator, the previously parsed left operand, and the newly parsed right operand. This new node becomes the new `left` for the next iteration of the loop.

3. **Termination**: When the loop exits (i.e., when there are no more binary operators following the initial expression), the function returns the final `ASTNode` that represents the entire parsed expression.

## Edge Cases

- **Empty Expression**: If there is no valid expression after `parsePower()`, the loop condition will fail immediately, and the function will return the result of `parsePower()`, which could be an empty or invalid node.
- **Mixed Types**: The function assumes that both operands of each binary operation are already parsed correctly into their respective AST nodes. Therefore, it does not handle mixed types directly; any type conversion must occur at a higher level of the compiler or during interpretation.
- **Precedence Handling**: Since multiplication, division, modulo, and floor division have the same precedence and are evaluated from left to right, the function's structure inherently handles this precedence without additional complexity.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. It uses methods like `current()` and `consume()` to interact with the tokenizer.
- **AST Construction**: The function constructs an abstract syntax tree (AST) by creating `ASTNode` objects and linking them together with `BinaryExpr`. This AST is used for further compilation steps such as semantic analysis and code generation.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function should integrate with error handling mechanisms to manage unexpected tokens or parsing errors gracefully.

By ensuring that all binary operations are correctly parsed and linked within the AST, the `parseMulDiv` function facilitates the subsequent stages of the compilation process, enabling accurate representation and manipulation of quantum expressions.