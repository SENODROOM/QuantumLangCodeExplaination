# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions include operations such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The primary goal of this function is to construct an Abstract Syntax Tree (AST) node representing the unary operation and its operand.

## Implementation Details
### Parameters
- None explicitly mentioned in the provided code snippet.

### Return Value
- Returns a unique pointer to an `ASTNode` object that represents the parsed unary expression.

### How It Works
The function begins by retrieving the current line number using `current().line`. It then checks for various unary operators sequentially:

1. **Increment (`++`)**:
   - If the current token type is `TokenType::PLUS_PLUS`, it consumes the token.
   - Recursively calls `parseUnary()` to parse the operand.
   - Creates a new `NumberLiteral` node with the value `1.0`.
   - Constructs an `AssignExpr` node with the operator `"+="`, moving the operand and the literal into it.
   - Returns the constructed `AssignExpr` node.

2. **Decrement (`--`)**:
   - Similar to the increment case, but uses the operator `"-"`.
   - Creates a new `NumberLiteral` node with the value `-1.0`.
   - Constructs an `AssignExpr` node with the operator `"-="`.

3. **Negation (`-`)**:
   - If the current token type is `TokenType::MINUS`, it consumes the token.
   - Constructs a `UnaryExpr` node with the operator `"-"` and moves the result of `parseUnary()` into it.
   - Returns the constructed `UnaryExpr` node.

4. **Logical NOT (`!`)**:
   - If the current token type is `TokenType::NOT`, it consumes the token.
   - Constructs a `UnaryExpr` node with the operator `"not"` and moves the result of `parseUnary()` into it.
   - Returns the constructed `UnaryExpr` node.

5. **Bitwise NOT (`~`)**:
   - If the current token type is `TokenType::BIT_NOT`, it consumes the token.
   - Constructs a `UnaryExpr` node with the operator `"~"` and moves the result of `parseUnary()` into it.
   - Returns the constructed `UnaryExpr` node.

6. **C-style Address-of (`&`)**:
   - If the current token type is `TokenType::BIT_AND`, it consumes the token.
   - Checks if the next token is an identifier containing double colons (`::`).
     - If true, it assumes the syntax is used to extract a member name from a namespace or class and constructs a `StringLiteral` node with the member name.
   - Otherwise, recursively calls `parseUnary()` to parse the operand.
   - Constructs an `AddressOfExpr` node with the moved operand and returns it.

7. **C-style Dereference (`*`)**:
   - If the current token type is `TokenType::STAR`, it consumes the token.
   - Recursively calls `parseUnary()` to parse the operand.
   - Constructs a `DerefExpr` node with the moved operand and returns it.

## Edge Cases
- **Empty Expression**: If there is no valid unary operator at the current position, the function will not consume any tokens and may lead to unexpected behavior if called without proper context.
- **Invalid Operand**: After consuming an unary operator, if `parseUnary()` fails to produce a valid operand, the function will return a malformed AST node.
- **Namespace/Class Member Extraction**: The address-of operator handling specifically looks for identifiers with double colons (`::`) to extract member names. This might fail if the syntax is incorrect or if the identifier does not exist.

## Interactions with Other Components
- **Token Stream**: The function interacts with the token stream through methods like `current()`, `check()`, and `consume()`. These methods provide access to the current token and allow the function to move forward in the stream after processing each token.
- **AST Construction**: The function relies on the creation of different types of AST nodes (`NumberLiteral`, `AssignExpr`, `UnaryExpr`, `AddressOfExpr`, `DerefExpr`) to represent the parsed unary expressions. These nodes are built using constructors that take the appropriate values and line numbers.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function likely integrates with broader error handling mechanisms within the compiler to manage cases where parsing fails due to invalid input or syntax errors.

This comprehensive approach ensures that the `parseUnary` function can correctly handle a variety of unary expressions, contributing to the robustness of the Quantum Language compiler's parsing capabilities.