# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions consist of a single operator followed by an operand, such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`) operators.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed unary expression.

## How It Works
The `parseUnary` function processes different types of unary operators based on the current token type. Here's how it works:

1. **Prefix Increment (`++`)**:
   - If the current token is `TokenType::PLUS_PLUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `AssignExpr` constructor, which represents the compound assignment `+=`. The right-hand side is set to a `NumberLiteral` with the value `1.0`.
   - This effectively increments the operand by `1`.

2. **Prefix Decrement (`--`)**:
   - Similar to prefix increment, but decrements the operand by `1` instead.

3. **Negation (`-`)**:
   - If the current token is `TokenType::MINUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then returns a new `ASTNode` using the `UnaryExpr` constructor, which represents the unary negation `-`.

4. **Logical NOT (`!`)**:
   - If the current token is `TokenType::NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It then returns a new `ASTNode` using the `UnaryExpr` constructor, which represents the logical NOT `not`.

5. **Bitwise NOT (`~`)**:
   - If the current token is `TokenType::BIT_NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It then returns a new `ASTNode` using the `UnaryExpr` constructor, which represents the bitwise NOT `~`.

6. **C-style Address-of (`&`)**:
   - If the current token is `TokenType::BIT_AND`, the function consumes the token.
   - It checks if the next token is an identifier containing `::` (indicating a namespace or class member).
   - If so, it extracts the member name and returns a new `ASTNode` using the `StringLiteral` constructor.
   - Otherwise, it recursively calls itself to parse the operand and returns a new `ASTNode` using the `AddressOfExpr` constructor, which represents the address-of operation.

7. **C-style Dereference (`*`)**:
   - If the current token is `TokenType::STAR`, the function consumes the token and recursively calls itself to parse the operand.
   - It returns a new `ASTNode` using the `DerefExpr` constructor, which represents the dereference operation.

## Edge Cases
- **No Operator**: If there is no valid unary operator at the current position, the function should call `parsePrimary()` to handle parsing of primary expressions (e.g., variables, literals).
- **Invalid Operand**: After consuming an operator, the function should ensure that a valid operand follows. If not, it should raise a syntax error.
- **Namespace/Class Member**: When handling the address-of operator (`&`), the function must correctly identify and extract namespace or class member names.

## Interactions with Other Components
- **Lexer**: The `parseUnary` function relies on the lexer to provide tokens. It uses functions like `current()`, `check()`, and `consume()` to interact with the lexer.
- **AST Construction**: The function constructs abstract syntax tree nodes (`ASTNode`) to represent the parsed unary expressions. These nodes are used by subsequent stages of the compiler for further processing.
- **Error Handling**: If an invalid token sequence is encountered, the function should raise appropriate errors to alert the user about the syntax issue. This interaction is crucial for maintaining robustness in the compiler.

Overall, the `parseUnary` function plays a vital role in parsing unary expressions within the Quantum Language compiler, ensuring that these expressions are accurately represented in the AST for further compilation steps.