# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are defined using curly braces `{}` and consist of key-value pairs. This function plays a critical role in handling the syntax and structure of dictionary literals within the language.

### Why It Works This Way

The function uses a loop to iterate through the tokens until it encounters a closing brace `}` or reaches the end of the input. Inside the loop, it checks for different types of keys such as identifiers, quoted strings, numbers, and type keywords. If a key is followed immediately by a colon `:`, it treats the key as a standard string literal. If not, it assumes the key is shorthand and creates a corresponding AST node.

### Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided snippet, but it likely receives the current parser state and possibly additional context.

- **Return Value**:
  - Returns an instance of `DictLiteral`, which represents the parsed dictionary literal. The `DictLiteral` class contains a vector of pairs, where each pair consists of a key and its corresponding value.

### Edge Cases

- **Empty Dictionary**: If the parser encounters an empty dictionary `{}`, it should handle this case gracefully without errors.
- **Spread Operator**: The function supports the spread operator (`...`) to merge multiple dictionaries into one. If the spread operator is used, the function should correctly parse the subsequent expression and add it to the dictionary.
- **Unexpected Tokens**: The function should be robust against unexpected tokens that do not conform to the expected dictionary literal syntax.

### Interactions With Other Components

- **Token Stream**: The function interacts with the token stream to consume tokens and determine their type. It uses helper functions like `current()`, `expect()`, `consume()`, and `match()` to manage the token stream.
- **AST Node Creation**: When parsing keys and values, the function creates instances of `ASTNode` to represent them in the abstract syntax tree (AST). These nodes are then added to the `DictLiteral`.
- **Error Handling**: The function includes error handling mechanisms to ensure that any unexpected tokens or syntax errors are detected and reported appropriately.

Here's a more complete version of the function based on typical C++ parsing patterns:

```cpp
#include <vector>
#include <memory>
#include <string>

// Define the necessary classes and enums
class ASTNode {
public:
    enum class Type {
        STRING_LITERAL,
        NUMBER,
        IDENTIFIER,
        SPREAD_OPERATOR
    };

    struct StringLiteral {
        std::string value;
        int line;
    };

    ASTNode(Type type, const StringLiteral& literal, int line)
        : type(type), literal(literal), line(line) {}

private:
    Type type;
    StringLiteral literal;
    int line;
};

enum class TokenType {
    LBRACE,
    RBRACE,
    COMMA,
    COLON,
    IDENTIFIER,
    TYPE_STRING,
    NEWLINE
};

class Token {
public:
    TokenType type;
    std::string value;
    int line;

    Token(TokenType type, std::string value, int line)
        : type(type), value(value), line(line) {}
};

class Parser {
public:
    std::vector<Token> tokens;
    size_t pos = 0;

    ASTNodePtr parseDictLiteral() {
        int ln = current().line;
        expect(TokenType::LBRACE, "Expected '{'");
        skipNewlines();
        DictLiteral dict;

        while (!check(TokenType::RBRACE) && !atEnd()) {
            // Handle spread operator {...obj, key: val}
            if (check(TokenType::IDENTIFIER) && current().value == "...") {
                consume(); // Eat "..."
                auto spreadExpr = parseUnary();
                // Use a special sentinel: key = nullptr means spread
                dict.pairs.emplace_back(nullptr, std::move(spreadExpr));
                skipNewlines();
                if (!match(TokenType::COMMA)) break;
                skipNewlines();
                if (check(TokenType::RBRACE)) break;
                continue;
            }

            // Parse key
            ASTNodePtr key;
            bool isShorthand = false;
            if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type) || check(TokenType::TYPE_STRING)) {
                // Peek ahead — if next token after this is COLON, treat as bare string key
                size_t la = pos + 1;
                while (la < tokens.size() && tokens[la].type == TokenType::NEWLINE) la++;

                if (la < tokens.size() && tokens[la].type == TokenType::COLON) {
                    // Bare identifier key: firstName → StringLiteral "firstName"
                    auto keyName = consume().value;
                    key = std::make_unique<ASTNode>(ASTNode::Type::STRING_LITERAL, StringLiteral{keyName}, ln);
                } else if (la < tokens.size() && (tokens[la].type == TokenType::COMMA || tokens[la].type == TokenType::RBRACE)) {
                    // Shorthand property: { x } or { x, y }
                    auto keyName = consume().value;
                    key = std::make_unique<ASTNode>(ASTNode::Type::STRING_LITERAL, StringLiteral{keyName}, ln);
                    isShorthand = true;
                } else {
                    key = parseExp(); // Assuming parseExp() handles parsing expressions
                }

                // Skip newlines before expecting a colon or comma
                skipNewlines();

                // Check for colon to separate key and value
                if (!match(TokenType::COLON)) {
                    if (isShorthand) {
                        break; // Allow trailing commas in shorthand properties
                    } else