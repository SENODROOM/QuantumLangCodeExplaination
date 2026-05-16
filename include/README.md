# QuantumLanguage Compiler - Lexer.h

## Overview

The `Lexer.h` header file is a crucial component of the QuantumLanguage compiler, focusing on the lexical analysis phase. This phase involves breaking down the source code into individual tokens, which are then processed by subsequent phases of the compiler. The `Lexer` class is designed to handle the conversion of raw text into a structured format suitable for further compilation steps.

## Role in Compiler Pipeline

The `Lexer` plays a vital role in the QuantumLanguage compiler's pipeline. It takes the input source code as a string and converts it into a sequence of tokens. These tokens represent the smallest meaningful elements of the language, such as keywords, identifiers, operators, and literals. The output of the lexer is consumed by the parser, which constructs the abstract syntax tree (AST) based on these tokens.

## Key Design Decisions and Why

1. **Tokenization**: The `Lexer` class is responsible for converting the source code into tokens. Each token has a type (`TokenType`) and associated value. This process ensures that the compiler can accurately interpret the source code.

2. **Handling Keywords**: A static unordered map named `keywords` is used to store reserved words and their corresponding token types. This allows the lexer to quickly identify and categorize keywords during the tokenization process.

3. **Support for F-Strings**: The `pendingTokens_` vector is utilized to manage tokens related to f-string expansion. F-strings provide a way to embed expressions inside string literals, allowing dynamic content generation at compile time.

4. **C Preprocessor Macros**: The `defines_` unordered map stores macro definitions, mapping macro names to lists of replacement tokens. This feature enables the lexer to preprocess the source code before actual tokenization, expanding macros where necessary.

5. **Character Handling**: The `current()` and `peek()` methods allow the lexer to inspect characters at specific positions in the source code without advancing the position pointer. The `advance()` method moves the lexer forward one character, facilitating the reading of tokens.

6. **Skipping Whitespace and Comments**: The `skipWhitespace()` and `skipComment()` methods help in ignoring unnecessary whitespace and comments, ensuring that only relevant information is processed during tokenization.

7. **Reading Different Types of Tokens**: The `readNumber()`, `readString(char quote)`, `readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)`, and `readIdentifierOrKeyword()` methods cater to various token types, including numbers, strings, template literals, and identifiers/keywords.

8. **Reading Operators**: The `readOperator()` method identifies and categorizes different operator symbols, ensuring proper parsing and handling of expressions.

## Major Classes/Functions Overview

### Class: Lexer

- **Constructor**: `explicit Lexer(const std::string &source);`
  Initializes the lexer with the source code string.

- **Function: tokenize()**
  Converts the source code into a vector of tokens.

### Private Methods

- **char current() const;**
  Returns the current character being processed.

- **char peek(int offset = 1) const;**
  Returns the character at the specified offset without advancing the position pointer.

- **char advance();**
  Advances the lexer to the next character and returns it.

- **void skipWhitespace();**
  Skips over any whitespace characters in the source code.

- **void skipComment();**
  Skips over single-line comments starting with `//`.

- **void skipBlockComment();**
  Skips over multi-line comments enclosed within `/* */`.

- **Token readNumber();**
  Reads a numeric literal from the source code and returns it as a `Token`.

- **Token readString(char quote);**
  Reads a string literal from the source code using the specified quote character and returns it as a `Token`.

- **void readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol);**
  Reads a template literal from the source code and appends its tokens to the provided vector.

- **Token readIdentifierOrKeyword();**
  Reads an identifier or keyword from the source code and returns it as a `Token`.

- **Token readOperator();**
  Reads an operator symbol from the source code and returns it as a `Token`.

## Tradeoffs

1. **Complexity vs. Performance**: Supporting features like f-string expansion and C preprocessor macros adds complexity to the lexer but enhances the flexibility and power of the language. Balancing these features against performance considerations is a tradeoff.

2. **Maintainability vs. Extensibility**: Adding more sophisticated tokenization rules and support for additional language features improves extensibility but may reduce maintainability due to increased code complexity.

3. **Readability vs. Functionality**: While the lexer provides robust functionality for tokenizing complex source code, maintaining readability and simplicity in the implementation is crucial for long-term maintenance and debugging.

By carefully balancing these tradeoffs, the `Lexer` class ensures that the QuantumLanguage compiler can efficiently and accurately parse source code, paving the way for effective compilation and execution.