# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is responsible for parsing unary expressions. These expressions typically involve a single operator applied to an operand, such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The function ensures that these operators are correctly interpreted and transformed into appropriate Abstract Syntax Tree (AST) nodes.

## Parameters
- **None**: This function operates on the global state of the parser, which includes the current token being processed.

## Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an AST node representing the parsed unary expression. Depending on the type of unary operation encountered, different types of AST nodes are returned:
  - For prefix increment (`++`) and decrement (`--`), an `AssignExpr` node is returned.
  - For negation (`-`), logical NOT (`!`), and bitwise NOT (`~`), a `UnaryExpr` node is returned.
  - For the C-style address-of (`&`) operator, an `AddressOfExpr` node is returned.
  - For the C-style dereference (`*`) operator, a `DerefExpr` node is returned.

## Edge Cases
- **Empty Expression**: If the parser encounters an empty expression or reaches the end of the input while trying to parse a unary expression, it should handle this gracefully without causing a runtime error.
- **Invalid Operand**: If the operand following a unary operator is not valid (e.g., expecting an identifier but encountering a number), the parser should generate an appropriate error message indicating the syntax error.
- **Nested Unary Operations**: The function should be able to handle nested unary operations correctly, ensuring that each operator is properly associated with its operand.

## Interactions with Other Components
- **Token Stream**: The `parseUnary` function relies on the global token stream managed by the parser. It consumes tokens from this stream to identify and process unary operators.
- **Error Handling**: The function interacts with the error handling mechanism of the parser to report any syntax errors encountered during the parsing process.
- **AST Construction**: The function constructs AST nodes based on the parsed unary expressions. These nodes are then used by subsequent phases of the compiler (e.g., semantic analysis, code generation) to represent the abstract structure of the quantum program.

## Implementation Details
Here's a breakdown of how the `parseUnary` function handles different unary operators:

1. **Prefix Increment (`++`)**:
   - Checks if the current token is `TokenType::PLUS_PLUS`.
   - Consumes the token.
   - Recursively parses the operand using `parseUnary()`.
   - Creates a `NumberLiteral` node representing the value `1.0`.
   - Constructs an `AssignExpr` node with the operator `"+="`, linking the operand and the literal.
   - Returns the constructed `AssignExpr` node.

2. **Prefix Decrement (`--`)**:
   - Similar to prefix increment, but uses the operator `"-"=`.
   - Creates a `NumberLiteral` node with the value `-1.0`.

3. **Negation (`-`)**:
   - Checks if the current token is `TokenType::MINUS`.
   - Consumes the token.
   - Recursively parses the operand using `parseUnary()`.
   - Constructs a `UnaryExpr` node with the operator `"-"`.
   - Returns the constructed `UnaryExpr` node.

4. **Logical NOT (`!`)**:
   - Checks if the current token is `TokenType::NOT`.
   - Consumes the token.
   - Recursively parses the operand using `parseUnary()`.
   - Constructs a `UnaryExpr` node with the operator `"not"`.
   - Returns the constructed `UnaryExpr` node.

5. **Bitwise NOT (`~`)**:
   - Checks if the current token is `TokenType::BIT_NOT`.
   - Consumes the token.
   - Recursively parses the operand using `parseUnary()`.
   - Constructs a `UnaryExpr` node with the operator `"~"`.
   - Returns the constructed `UnaryExpr` node.

6. **C-style Address-of (`&`)**:
   - Checks if the current token is `TokenType::BIT_AND`.
   - Consumes the token.
   - If the next token is an identifier containing a scope resolution operator (`::`), extracts the member name and creates a `StringLiteral` node.
   - Otherwise, recursively parses the operand using `parseUnary()`.
   - Constructs an `AddressOfExpr` node with the operand.
   - Returns the constructed `AddressOfExpr` node.

7. **C-style Dereference (`*`)**:
   - Checks if the current token is `TokenType::STAR`.
   - Consumes the token.
   - Recursively parses the operand using `parseUnary()`.
   - Constructs a `DerefExpr` node with the operand.
   - Returns the constructed `DerefExpr` node.

By carefully handling each unary operator and constructing the corresponding AST nodes, the `parseUnary` function enables the Quantum Language compiler to accurately represent and analyze unary expressions within the quantum program.