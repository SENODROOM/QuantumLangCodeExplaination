# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions consist of a single operator followed by an operand, such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`). The function handles both prefix and postfix forms of these operators.

## Parameters
- None

## Return Value
- A unique pointer to an `ASTNode` representing the parsed unary expression.

## Edge Cases
- Handles both prefix and postfix forms of unary operators.
- Returns the operand unchanged for the unary plus (`+`) operator, which is a no-op.
- Correctly parses nested unary expressions.

## Interactions with Other Components
- **Tokenizer**: The function uses the tokenizer to check the current token type and consume tokens as needed.
- **ASTBuilder**: It constructs an abstract syntax tree (AST) node for each unary expression using the `ASTNode` class.
- **Error Handling**: If an unexpected token is encountered, the function may throw an exception or handle errors gracefully depending on the implementation.

## Implementation Details
The function starts by recording the current line number (`ln`). It then checks for various unary operators and processes them accordingly:

1. **Prefix Increment (`++`)**:
   - Consumes the `PLUS_PLUS` token.
   - Recursively calls `parseUnary()` to get the operand.
   - Creates a `NumberLiteral` node with the value `1.0`.
   - Constructs an `AssignExpr` node with the operation `"+="`, moving the operand and the literal into it.
   
2. **Prefix Decrement (`--`)**:
   - Similar to prefix increment but with the operation `"="-`.

3. **Negation (`-`)**:
   - Consumes the `MINUS` token.
   - Recursively calls `parseUnary()` to get the operand.
   - Constructs a `UnaryExpr` node with the operation `"-"`, moving the operand into it.

4. **Logical NOT (`!`)**:
   - Consumes the `NOT` token.
   - Recursively calls `parseUnary()` to get the operand.
   - Constructs a `UnaryExpr` node with the operation `"not"`, moving the operand into it.

5. **Bitwise NOT (`~`)**:
   - Consumes the `BIT_NOT` token.
   - Recursively calls `parseUnary()` to get the operand.
   - Constructs a `UnaryExpr` node with the operation `"~"`, moving the operand into it.

6. **C-style Address-of Operator (`&`)**:
   - Consumes the `BIT_AND` token.
   - Checks if the next token is an identifier containing double colons (`::`), indicating a namespace or scope resolution.
   - If so, extracts the member name after the last colon and returns it as a `StringLiteral`.
   - Otherwise, recursively calls `parseUnary()` to get the operand.
   - Constructs an `AddressOfExpr` node with the operand.

7. **C-style Dereference Operator (`*`)**:
   - Consumes the `STAR` token.
   - Recursively calls `parseUnary()` to get the operand.
   - Constructs a `DerefExpr` node with the operand.

Each case ensures that the correct AST node is constructed based on the unary operator found and the subsequent operand. This allows for the proper representation and evaluation of unary expressions within the quantum language's execution environment.