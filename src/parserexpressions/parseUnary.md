# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions include operations such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The function ensures that these operators are correctly parsed and converted into their corresponding Abstract Syntax Tree (AST) nodes.

## Parameters
- None

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed unary expression.

## How It Works
The `parseUnary` function processes different types of unary operators based on the current token type. Here’s how each operator is handled:

1. **Prefix Increment (`++`)**:
   - If the current token is `TokenType::PLUS_PLUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then creates a new `ASTNode` using the `AssignExpr` constructor, which represents the operation `+=`. This effectively increments the operand by 1.
   - The line number is preserved for accurate error reporting or debugging.

2. **Prefix Decrement (`--`)**:
   - Similar to prefix increment, but instead of creating an `AssignExpr` with `+=`, it creates an `AssignExpr` with `-=`. This decrements the operand by 1.

3. **Negation (`-`)**:
   - For the negation operator, if the current token is `TokenType::MINUS`, the function consumes the token and recursively calls itself to parse the operand.
   - It then returns a new `ASTNode` using the `UnaryExpr` constructor, indicating that the operand should be negated.

4. **Logical NOT (`!`)**:
   - If the current token is `TokenType::NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It returns a new `ASTNode` using the `UnaryExpr` constructor, indicating that the operand should undergo a logical NOT operation.

5. **Bitwise NOT (`~`)**:
   - For the bitwise NOT operator, if the current token is `TokenType::BIT_NOT`, the function consumes the token and recursively calls itself to parse the operand.
   - It returns a new `ASTNode` using the `UnaryExpr` constructor, indicating that the operand should undergo a bitwise NOT operation.

6. **C-style Address-of (`&`)**:
   - If the current token is `TokenType::BIT_AND`, the function consumes the token and recursively calls itself to parse the operand.
   - It checks if the next token is an identifier containing the scope resolution operator (`::`). If so, it extracts the member name after the last `::`.
   - Otherwise, it returns a new `ASTNode` using the `AddressOfExpr` constructor, indicating that the operand's address should be taken.

7. **C-style Dereference (`*`)**:
   - If the current token is `TokenType::STAR`, the function consumes the token and recursively calls itself to parse the operand.
   - It returns a new `ASTNode` using the `DerefExpr` constructor, indicating that the operand should be dereferenced.

## Edge Cases
- **Empty Expression**: If there is no valid unary expression at the current position, the function will not consume any tokens and will simply return `nullptr`.
- **Invalid Operand**: If the operand following a unary operator is invalid, the function will throw an exception indicating a syntax error.
- **Nested Operators**: The function can handle nested unary operators, ensuring that they are parsed correctly according to the rules of operator precedence.

## Interactions with Other Components
- **Lexer**: The `parseUnary` function relies on the lexer to provide the current token and subsequent tokens. The lexer must correctly identify and categorize tokens such as identifiers, numbers, and operators.
- **Error Handling**: During parsing, the function may encounter errors (e.g., missing operands, invalid tokens). In such cases, it interacts with the error handling component to report appropriate error messages.
- **AST Construction**: The parsed unary expressions are constructed as AST nodes, which are then used by other parts of the compiler for further processing, such as semantic analysis, code generation, and optimization.

By understanding how `parseUnary` functions, developers can better grasp the structure and behavior of unary expressions in the Quantum Language compiler, enabling them to write more efficient and correct code.