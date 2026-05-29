# readString Function

## Overview

The `readString` function is responsible for parsing and extracting string literals from the source code during the lexical analysis phase of the Quantum Language compiler. This function ensures that string literals are accurately recognized and handled, including processing escape sequences within the strings.

### Why It Works This Way

This implementation of `readString` follows a straightforward approach to parse string literals:

1. **Initialization**: The function records the starting line and column positions (`startLine`, `startCol`) where the string begins.
2. **Skipping Opening Quote**: The first character in a string literal is always a quotation mark (`'` or `"`). The function skips over this opening quote using the `advance()` method.
3. **Reading Characters Inside String**:
   - The function enters a loop that continues until it encounters the closing quotation mark or reaches the end of the source code.
   - If the current character is an escape sequence (`\`), it processes the following character according to standard C++ escape rules (e.g., `\n` for newline, `\t` for tab).
   - For regular characters, the function simply advances to the next character and appends it to the string (`str`).
4. **Handling Edge Cases**:
   - If the function reaches the end of the source code without finding the closing quotation mark, it throws a `QuantumError` indicating an unterminated string literal.
5. **Skipping Closing Quote**: After successfully reading the string up to the closing quotation mark, the function skips over this closing quote.
6. **Returning Token**: Finally, the function returns a `Token` object representing the parsed string literal, along with its type (`STRING`), content (`str`), and original position in the source code (`startLine`, `startCol`).

### Parameters/Return Value

- **Parameters**:
  - None explicitly defined in the provided snippet; however, it implicitly relies on global variables like `line`, `col`, `src`, and `pos`.
  
- **Return Value**:
  - Returns a `Token` object containing the type `TokenType::STRING`, the extracted string literal, and the starting line and column positions.

### Interactions With Other Components

- **Lexer Class**: The `readString` function is part of the `Lexer` class, which handles the overall lexical analysis process.
- **Source Code**: The function operates on the `src` string, which represents the entire source code being compiled.
- **Position Tracking**: The function uses global variables `line`, `col`, and `pos` to keep track of the current position in the source code, allowing for accurate error reporting and tokenization.

### Example Usage

Here’s how you might use the `readString` function within the context of a larger lexer:

```cpp
// Assuming Lexer is a class with necessary member functions and variables
class Lexer {
public:
    Token readNextToken();

private:
    void advance();
    char current();
    std::string src;
    size_t pos;
    int line;
    int col;
};

Token Lexer::readNextToken() {
    // ... other token reading logic ...
    
    if (current() == '"' || current() == '\'') {
        char quote = current();
        return readString(quote); // Pass the quote character to readString
    }

    // ... other token reading logic ...
}

Token Lexer::readString(char quote) {
    int startLine = line, startCol = col;
    advance(); // Skip opening quote
    std::string str;
    while (pos < src.size() && current() != quote) {
        if (current() == '\\') {
            advance();
            switch (current()) {
                case 'n': str += '\n'; break;
                case 't': str += '\t'; break;
                case 'r': str += '\r'; break;
                case '\\': str += '\\'; break;
                case '\'': str += '\''; break;
                case '"': str += '"'; break;
                case '0': str += '\0'; break;
                default: str += current();
            }
            advance();
        } else {
            str += advance();
        }
    }
    if (pos >= src.size())
        throw QuantumError("LexError", "Unterminated string literal", startLine);
    advance(); // Skip closing quote
    return Token(TokenType::STRING, str, startLine, startCol);
}
```

In this example, the `readNextToken` function checks if the current character is a quotation mark and then calls `readString` to handle the extraction of the string literal. The `readString` function processes the string according to the specified rules and returns a `Token` object.