# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions include operations such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The function ensures that these operators are correctly interpreted and transformed into their corresponding Abstract Syntax Tree (AST) nodes.

## Parameters
- **None**: This function operates directly on the global state of the parser, which includes the current token being processed.

## Return Value
- **Unique Pointer to ASTNode**: The function returns a unique pointer to an `ASTNode` representing the parsed unary expression. Depending on the type of unary operator encountered, different types of `ASTNode` subclasses are returned:
  - For prefix increment (`++`) and decrement (`--`), it returns an `AssignExpr` node.
  - For negation (`-`), logical NOT (`!`), and bitwise NOT (`~`), it returns a `UnaryExpr` node.
  - For the C-style address-of (`&`) operator, it returns an `AddressOfExpr` node.
  - For the C-style dereference (`*`) operator, it returns a `DerefExpr` node.

## Edge Cases
- **Empty Expression**: If the parser encounters an empty expression or reaches the end of input while expecting a unary operator, it may throw an error or handle it gracefully based on the implementation details.
- **Invalid Operator**: If the parser encounters an invalid unary operator (e.g., `@@`, `$`), it should raise an appropriate error indicating syntax issues.
- **Nested Expressions**: The function must correctly handle nested unary expressions. For example, `++a * b` should be parsed as `(++a) * b`.

## Interactions with Other Components
- **Token Stream**: The `parseUnary` function relies on the global token stream managed by the parser. It checks the current token's type to determine whether a unary operator is present and consumes tokens accordingly.
- **Error Handling**: The function interacts with the error handling mechanism of the compiler. If an unexpected token is encountered, it raises an error using the provided line number information.
- **Expression Parsing**: The function calls itself recursively to parse nested unary expressions. This allows it to build complex AST structures from simple ones.
- **Symbol Table**: When parsing the address-of (`&`) operator, the function checks the symbol table to ensure that the identifier following the operator is valid. If not, it raises an error.

## Implementation Details
Here’s a breakdown of how each unary operator is handled:

1. **Prefix Increment (`++`)**:
   ```cpp
   if (check(TokenType::PLUS_PLUS)) {
       consume();
       auto operand = parseUnary();
       auto one = std::make_unique<ASTNode>(NumberLiteral{1.0}, ln);
       return std::make_unique<ASTNode>(AssignExpr{"+=", std::move(operand), std::move(one)}, ln);
   }
   ```
   - Checks if the current token is a prefix increment (`++`).
   - Consumes the token.
   - Recursively parses the operand.
   - Creates a `NumberLiteral` node representing the value `1`.
   - Returns an `AssignExpr` node with the operation `"+="` between the operand and the literal `1`.

2. **Prefix Decrement (`--`)**:
   ```cpp
   if (check(TokenType::MINUS_MINUS)) {
       consume();
       auto operand = parseUnary();
       auto one = std::make_unique<ASTNode>(NumberLiteral{1.0}, ln);
       return std::make_unique<ASTNode>(AssignExpr{"-=", std::move(operand), std::move(one)}, ln);
   }
   ```
   - Similar to prefix increment but uses the `-=` operation instead.

3. **Negation (`-`)**:
   ```cpp
   if (check(TokenType::MINUS)) {
       consume();
       return std::make_unique<ASTNode>(UnaryExpr{"-", parseUnary()}, ln);
   }
   ```
   - Checks if the current token is a negation (`-`).
   - Consumes the token.
   - Recursively parses the operand.
   - Returns a `UnaryExpr` node with the operation `"-"`.

4. **Logical NOT (`!`)**:
   ```cpp
   if (check(TokenType::NOT)) {
       consume();
       return std::make_unique<ASTNode>(UnaryExpr{"not", parseUnary()}, ln);
   }
   ```
   - Checks if the current token is a logical NOT (`!`).
   - Consumes the token.
   - Recursively parses the operand.
   - Returns a `UnaryExpr` node with the operation `"not"`.

5. **Bitwise NOT (`~`)**:
   ```cpp
   if (check(TokenType::BIT_NOT)) {
       consume();
       return std::make_unique<ASTNode>(UnaryExpr{"~", parseUnary()}, ln);
   }
   ```
   - Checks if the current token is a bitwise NOT (`~`).
   - Consumes the token.
   - Recursively parses the operand.
   - Returns a `UnaryExpr` node with the operation `"~"`.

6. **C-style Address-of (`&`)**:
   ```cpp
   if (check(TokenType::BIT_AND)) {
       consume();
       int ln2 = current().line;
       if (check(TokenType::IDENTIFIER) && current().value.find("::") != std::string::npos) {
           std::string val = consume().value;
           size_t pos = val.rfind("::");
           std::string member = val.substr(pos + 2);
           return