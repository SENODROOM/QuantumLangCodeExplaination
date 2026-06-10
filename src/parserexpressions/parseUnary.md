# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions consist of a single operator followed by an operand, such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`).

## Parameters
- None

## Return Value
- A unique pointer to an `ASTNode` representing the parsed unary expression.

## How It Works
The `parseUnary` function processes different types of unary operators and constructs the corresponding abstract syntax tree (AST) nodes:

1. **Prefix Increment (`++`)**:
   - If the current token is `TokenType::PLUS_PLUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `AssignExpr` constructor, which represents an assignment operation with the `"+="` operator and a constant integer literal `1.0`.
   - The line number of the original token is preserved.

2. **Prefix Decrement (`--`)**:
   - Similar to prefix increment, but uses the `"-"=` operator instead.

3. **Negation (`-`)**:
   - If the current token is `TokenType::MINUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `UnaryExpr` constructor, which represents a unary negation operation with the `"-"` operator.
   - The line number of the original token is preserved.

4. **Logical Not (`!`)**:
   - If the current token is `TokenType::NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `UnaryExpr` constructor, which represents a logical not operation with the `"not"` operator.
   - The line number of the original token is preserved.

5. **Bitwise Not (`~`)**:
   - If the current token is `TokenType::BIT_NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `UnaryExpr` constructor, which represents a bitwise not operation with the `"~"` operator.
   - The line number of the original token is preserved.

6. **C-style Address-of (`&`)**:
   - If the current token is `TokenType::BIT_AND`, the function consumes the token and recursively calls itself to parse the operand.
   - If the next token is an identifier containing double colons (`::`), it extracts the member name after the last colon and returns a `StringLiteral` node representing that member.
   - Otherwise, it creates a new `ASTNode` using the `AddressOfExpr` constructor, which represents an address-of operation.
   - The line number of the original token is preserved.

7. **C-style Dereference (`*`)**:
   - If the current token is `TokenType::STAR`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `DerefExpr` constructor, which represents a dereference operation.
   - The line number of the original token is preserved.

8. **No-op Unary Plus (`+`)**:
   - If the current token is `TokenType::PLUS`, the function consumes the token and recursively calls itself to parse the operand.
   - Since unary plus is a no-operation (i.e., `+1` is equivalent to `1`), it simply discards the `+` token and returns the result of parsing the operand.

## Edge Cases
- **Empty Input**: If there are no tokens left to parse, the function will eventually reach a point where it cannot match any of the expected unary operators, leading to a parsing error.
- **Invalid Operand**: If the operand following a unary operator is invalid or incomplete, the recursive call to `parseUnary` may fail, resulting in a parsing error.
- **Nested Unary Operators**: The function handles nested unary operators correctly by recursively calling itself until it reaches a valid operand.

## Interactions with Other Components
- **Token Stream**: The function interacts with the token stream provided by the parser to identify and consume unary operators and their operands.
- **Abstract Syntax Tree (AST)**: The function constructs AST nodes based on the parsed unary expressions, which are then used by other parts of the compiler for further processing, such as semantic analysis and code generation.
- **Error Handling**: The function includes checks and consumes tokens to ensure proper parsing. If an unexpected token is encountered, it leads to a parsing error, which is handled by higher-level components of the compiler.

This comprehensive approach ensures that the `parseUnary` function can accurately parse various forms of unary expressions in the Quantum Language, providing a solid foundation for subsequent stages of compilation.