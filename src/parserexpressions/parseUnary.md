# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is responsible for parsing unary expressions. These include operations such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The function ensures that these operators are correctly parsed and transformed into their corresponding Abstract Syntax Tree (AST) nodes.

## Parameters
- None explicitly declared in the provided code snippet.

## Return Value
- Returns an `std::unique_ptr<ASTNode>` representing the parsed unary expression.

## Edge Cases
1. **Increment (`++`)**: If the token is `TokenType::PLUS_PLUS`, the function consumes the token and recursively calls itself to parse the operand. It then creates an `AssignExpr` node with the `"+="` operator and a `NumberLiteral` of `1.0`.
   
2. **Decrement (`--`)**: Similar to increment, but uses the `"-"` operator instead.

3. **Negation (`-`)**: If the token is `TokenType::MINUS`, the function consumes the token and recursively calls itself to parse the operand. It then returns a `UnaryExpr` node with the `"-"` operator.

4. **Logical NOT (`!`)**: If the token is `TokenType::NOT`, the function consumes the token and recursively calls itself to parse the operand. It then returns a `UnaryExpr` node with the `"not"` operator.

5. **Bitwise NOT (`~`)**: If the token is `TokenType::BIT_NOT`, the function consumes the token and recursively calls itself to parse the operand. It then returns a `UnaryExpr` node with the `"~"` operator.

6. **Address-of (`&`)**: If the token is `TokenType::BIT_AND`, the function consumes the token. If the next token is an identifier containing double colons (`::`), it extracts the member name after the last colon and returns a `StringLiteral` node. Otherwise, it recursively calls itself to parse the operand and returns an `AddressOfExpr` node.

7. **Dereference (`*`)**: If the token is `TokenType::STAR`, the function consumes the token and recursively calls itself to parse the operand. It then returns a `DerefExpr` node.

## Interactions with Other Components
- **Token Stream**: The function interacts with the token stream to identify and consume tokens related to unary operators. This involves calling functions like `current()`, `check(TokenType type)`, and `consume()` to manage the token flow.
  
- **Recursive Parsing**: For some operators (e.g., `++`, `--`, `-`, `!`, `~`, `&`, `*`), the function makes recursive calls to `parseUnary()` to handle nested or subsequent expressions.

- **AST Construction**: Once a unary operation is identified and its operand is parsed, the function constructs the appropriate AST node (`AssignExpr`, `UnaryExpr`, `AddressOfExpr`, `DerefExpr`) using the parsed operand and any additional information needed (like literals).

- **Error Handling**: Although not shown in the snippet, the function likely includes error handling mechanisms to ensure that invalid unary expressions are detected and reported appropriately.

This function plays a crucial role in the parser's ability to accurately interpret and construct the AST for unary expressions, which are fundamental building blocks in many programming languages including Quantum Language.