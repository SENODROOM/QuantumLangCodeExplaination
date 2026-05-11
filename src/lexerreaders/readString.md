# `readString` Function

## Overview

The `readString` function is responsible for lexically analyzing and extracting string literals from the source code being processed by the Quantum Language compiler's lexer. This function ensures that string literals are correctly identified and handled, including support for escape sequences within the strings.

### Why It Works This Way

The function operates by iterating through the source code starting from the position immediately after the opening quotation mark (`"`). It reads characters until it encounters the closing quotation mark or reaches the end of the source code. During this process, it checks for escape sequences prefixed by a backslash (`\`). If an escape sequence is found, it interprets the character following the backslash according to the standard rules for escape sequences in C++, such as `\n` for newline, `\t` for tab, etc. If the escape sequence is not recognized, it simply appends the character to the string.

### Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A `Token` object representing the extracted string literal. The `Token` constructor takes several parameters:
    - `TokenType::STRING`: Indicates that the token type is a string.
    - `str`: The content of the string literal.
    - `startLine`: The line number where the string literal begins.
    - `startCol`: The column number where the string literal begins.

### Edge Cases

1. **Unterminated String Literal**: If the function reaches the end of the source code without encountering the closing quotation mark, it throws a `QuantumError` with the message "Unterminated string literal" and the starting line number of the string literal.

2. **Empty String Literal**: An empty string literal (i.e., `"")` will be successfully parsed and returned as a `Token`.

3. **Escape Sequences**: The function handles various escape sequences like `\n`, `\t`, `\r`, `\\`, `'`, `"`, and `\0`. Any unrecognized escape sequence results in appending the character directly to the string.

4. **Backslash at End of String**: If the backslash is the last character before the closing quotation mark, it is treated as part of the string rather than an escape sequence.

### Interactions With Other Components

- **Lexer Class**: The `readString` function is typically called within the `Lexer` class when the lexer encounters a double-quote character (`"`), indicating the beginning of a string literal. After reading the string, the lexer moves to the next token.

- **Source Code Handling**: The function uses the `src` member variable of the `Lexer` class, which contains the entire source code being analyzed. It also updates the `line` and `col` variables to keep track of the current parsing position.

- **Error Handling**: If an unterminated string literal is detected, the function throws a `QuantumError` using the `throw` statement. This error is then caught and handled by higher-level components of the compiler.

Here is the updated implementation of the `readString` function:

```cpp
#include <iostream>
#include <string>

// Define possible token types
enum TokenType { STRING };

// Define a custom exception for quantum errors
class QuantumError : public std::exception {
public:
    QuantumError(const std::string& type, const std::string& message, int lineNumber)
        : type(type), message(message), lineNumber(lineNumber) {}

    const char* what() const noexcept override {
        return message.c_str();
    }

private:
    std::string type;
    std::string message;
    int lineNumber;
};

// Define a simple Token structure
struct Token {
    TokenType type;
    std::string value;
    int line;
    int column;

    Token(TokenType t, std::string v, int l, int c) : type(t), value(v), line(l), column(c) {}
};

// Lexer class definition
class Lexer {
public:
    Lexer(const std::string& input) : src(input), pos(0), line(1), col(1) {}

    Token nextToken() {
        skipWhitespace();
        if (pos >= src.size()) return Token(TokenType::EOF, "", line, col);

        char ch = current();
        if (ch == '"') {
            return readString();
        } else if (isAlpha(ch)) {
            return readIdentifier();
        } else if (isdigit(ch)) {
            return readNumber();
        } else if (ch == '+') {
            return makeToken(TokenType::PLUS);
        } else if (ch == '-') {
            return makeToken(TokenType::MINUS);
        } else if (ch == '*') {
            return makeToken(TokenType::ASTERISK);
        } else if (ch == '/') {
            return makeToken(TokenType::SLASH);
        } else if (ch == '(') {
            return makeToken(TokenType::LPAREN);
        } else if (ch == ')') {
            return makeToken(TokenType::RPAREN);
        } else if (ch == '{') {
            return makeToken(TokenType::LBRACE);
        } else if (ch == '}') {
            return makeToken(TokenType::RBRACE);
        } else if (ch == ';') {
            return makeToken(TokenType::SEMICOLON);
        } else {
            throw QuantumError("SyntaxError", "Unexpected character: " + std::string(1, ch), line);
        }
    }

private:
    std::string src; // Source code
    size_t pos;      // Current position in the source code
    int line;         // Current line number
    int col;          // Current column number

    void advance() {
        ++pos;
        if (src[pos] == '\n') {
            ++line;
            col = 1;
        } else {
            ++col