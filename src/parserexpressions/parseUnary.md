# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions include operations such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereferencing (`*`). The function ensures that these operators are correctly identified and handled within the context of the quantum programming language syntax.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an ASTNode representing the parsed unary expression.

## How It Works
The `parseUnary` function processes tokens sequentially to identify and construct unary expressions. Here’s how it works:

1. **Increment (`++`)**:
   - If the current token is `TokenType::PLUS_PLUS`, it consumes the token and recursively calls itself to parse the operand.
   - Constructs an `AssignExpr` with the operator `"+="`, the operand, and a new `NumberLiteral` node with the value `1.0`.
   - Returns the constructed `AssignExpr`.

2. **Decrement (`--`)**:
   - Similar to increment, but constructs an `AssignExpr` with the operator `"-"=` instead.

3. **Negation (`-`)**:
   - If the current token is `TokenType::MINUS`, it consumes the token and recursively calls itself to parse the operand.
   - Constructs a `UnaryExpr` with the operator `"-"` and the operand.
   - Returns the constructed `UnaryExpr`.

4. **Logical NOT (`!`)**:
   - If the current token is `TokenType::NOT`, it consumes the token and recursively calls itself to parse the operand.
   - Constructs a `UnaryExpr` with the operator `"not"` and the operand.
   - Returns the constructed `UnaryExpr`.

5. **Bitwise NOT (`~`)**:
   - If the current token is `TokenType::BIT_NOT`, it consumes the token and recursively calls itself to parse the operand.
   - Constructs a `UnaryExpr` with the operator `"~"` and the operand.
   - Returns the constructed `UnaryExpr`.

6. **C-style Address-of (`&`)**:
   - If the current token is `TokenType::BIT_AND`, it consumes the token and checks if the next token is an identifier containing `::`.
     - If so, it extracts the member name after `::` and returns a `StringLiteral` node with this member name.
   - Otherwise, it recursively calls itself to parse the operand.
   - Constructs an `AddressOfExpr` with the operand.
   - Returns the constructed `AddressOfExpr`.

7. **C-style Dereference (`*`)**:
   - If the current token is `TokenType::STAR`, it consumes the token and recursively calls itself to parse the operand.
   - Constructs a `DerefExpr` with the operand.
   - Returns the constructed `DerefExpr`.

## Edge Cases
- **Empty Expression**: If there is no valid unary operator followed by an operand, the function will not consume any tokens and may lead to errors or unexpected behavior depending on the calling context.
- **Invalid Operand**: After consuming an unary operator, if the subsequent token is not a valid operand (e.g., another unary operator without an operand), the function will throw an error indicating a syntax issue.
- **Nested Unary Expressions**: The function can handle nested unary expressions, such as `++(*ptr)`, where it will correctly interpret each unary operator and its associated operand.

## Interactions with Other Components
- **Lexer**: The function relies on the lexer to provide tokens for parsing. Each call to `current()` retrieves the current token being processed, and `consume()` advances to the next token.
- **ASTBuilder**: The function uses the `ASTNode` class to build the abstract syntax tree (AST). It creates nodes like `AssignExpr`, `UnaryExpr`, `AddressOfExpr`, and `DerefExpr` based on the parsed unary expressions.
- **Error Handling**: The function includes basic error handling to ensure that only valid unary expressions are parsed. If an invalid sequence of tokens is encountered, it throws an exception indicating a syntax error.

Overall, the `parseUnary` function plays a crucial role in the Quantum Language compiler by accurately parsing unary expressions and constructing them into the AST. This allows for proper interpretation and execution of quantum programs involving these operators.