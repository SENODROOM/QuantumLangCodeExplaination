# `parseUnary` Function

## Purpose
The `parseUnary` function in the Quantum Language compiler is designed to parse unary expressions. These expressions consist of a single operator followed by an operand, such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). The function ensures that these operators are correctly interpreted and transformed into their corresponding Abstract Syntax Tree (AST) nodes.

## Parameters
- **None**: The `parseUnary` function operates on the global state of the parser, which includes the current token being processed and the ability to consume tokens.

## Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an AST node representing the parsed unary expression. Depending on the type of unary operator encountered, different types of AST nodes are returned:
  - For prefix increment (`++`) and decrement (`--`), it returns an `AssignExpr` node.
  - For postfix increment (`i++`) and decrement (`i--`), it returns an `IncDecExpr` node.
  - For negation (`-`), logical NOT (`!`), and bitwise NOT (`~`), it returns a `UnaryExpr` node.
  - For address-of (`&`) and dereference (`*`), it returns an `AddressOfExpr` and `DerefExpr` node respectively.

## Edge Cases
- **Invalid Unary Operator**: If the current token is not a valid unary operator, the function will throw an error indicating an unexpected token.
- **Postfix Increment/Decrement**: The function currently only handles prefix increment and decrement. Handling postfix versions would require additional logic to manage the order of operations correctly.
- **Namespace Qualification**: In the case of the address-of operator (`&`), the function checks if the identifier contains a scope resolution operator (`::`). If so, it extracts the member name and returns a `StringLiteral` node instead of an `AddressOfExpr`. This behavior may need to be adjusted based on the language's rules regarding namespace qualification and address-of operations.

## Interactions with Other Components
- **Token Stream**: The `parseUnary` function relies on the global token stream managed by the parser. It consumes tokens as part of its operation to progress through the input source code.
- **Error Handling**: Errors during parsing are handled by throwing exceptions. The calling component should catch these exceptions and handle them appropriately, possibly reporting errors to the user or continuing execution in a controlled manner.
- **AST Construction**: The function constructs AST nodes based on the parsed unary expressions. These nodes are then used by subsequent stages of the compilation process, such as semantic analysis and code generation, to build the final executable representation of the program.

## Implementation Details
The implementation of `parseUnary` is structured to handle each type of unary operator separately. For example, when encountering a prefix increment (`++`), the function consumes the token, recursively parses the operand, creates a `NumberLiteral` node with the value `1.0`, and then constructs an `AssignExpr` node with the operator `"+="`. This approach allows for clear separation of concerns and easy expansion to support additional unary operators in the future.

Here is a simplified version of how the function might be implemented:

```cpp
std::unique_ptr<ASTNode> parseUnary() {
    int ln = current().line;
    
    // Handle prefix operators
    if (check(TokenType::PLUS_PLUS)) {
        consume();
        auto operand = parseUnary();
        auto one = std::make_unique<ASTNode>(NumberLiteral{1.0}, ln);
        return std::make_unique<ASTNode>(AssignExpr{"+=", std::move(operand), std::move(one)}, ln);
    }
    if (check(TokenType::MINUS_MINUS)) {
        consume();
        auto operand = parseUnary();
        auto one = std::make_unique<ASTNode>(NumberLiteral{1.0}, ln);
        return std::make_unique<ASTNode>(AssignExpr{"-=", std::move(operand), std::move(one)}, ln);
    }
    if (check(TokenType::MINUS)) {
        consume();
        return std::make_unique<ASTNode>(UnaryExpr{"-", parseUnary()}, ln);
    }
    if (check(TokenType::PLUS)) {
        consume();
        return parseUnary(); // Discard the '+'
    }
    if (check(TokenType::NOT)) {
        consume();
        return std::make_unique<ASTNode>(UnaryExpr{"not", parseUnary()}, ln);
    }
    if (check(TokenType::BIT_NOT)) {
        consume();
        return std::make_unique<ASTNode>(UnaryExpr{"~", parseUnary()}, ln);
    }
    
    // Handle address-of and dereference operators
    if (check(TokenType::BIT_AND)) {
        consume();
        int ln2 = current().line;
        if (check(TokenType::IDENTIFIER) && current().value.find("::") != std::string::npos) {
            std::string val = consume().value;
            size_t pos = val.rfind("::");
            std::string member = val.substr(pos + 2);
            return std::make_unique<ASTNode>(StringLiteral{member}, ln2);
        }
        auto operand = parseUnary();
        return std::make_unique<ASTNode>(AddressOfExpr{std::move(operand)}, ln2);
    }
    if (check(TokenType::STAR)) {
        consume();
        int ln2 = current().line;
        auto operand = parseUnary();
        return std::make_unique<ASTNode>(DerefExpr{std::move(operand)}, ln2);
    }
    
    // If none of the above operators are found, consume the token and call