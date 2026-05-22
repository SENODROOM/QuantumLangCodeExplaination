# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is a crucial component of the QuantumLanguage compiler, focusing on the lexical analysis phase. This phase involves breaking down the source code into individual tokens that can be processed further in the compilation pipeline. The `Lexer` class plays a pivotal role in managing this process efficiently and accurately.

## Role in Compiler Pipeline

The `Lexer` class operates at the beginning of the compiler pipeline, converting raw source code into a stream of tokens. These tokens represent the smallest meaningful units in the language, such as keywords, identifiers, numbers, strings, and operators. By tokenizing the input, the lexer facilitates subsequent phases like parsing and semantic analysis, which rely on structured data rather than plain text.

### Key Design Decisions and Why

1. **Tokenization Rules**: The lexer must adhere to strict rules to correctly identify and categorize tokens. For instance, it needs to distinguish between identifiers and keywords, handle different types of literals (numbers, strings), and recognize operators and delimiters. This ensures that the compiler can interpret the code accurately.

2. **Error Handling**: Efficient error handling is critical during the lexical analysis phase. The lexer should be able to detect syntax errors early and provide clear messages to help developers correct their code. This enhances the overall reliability and maintainability of the compiler.

3. **State Management**: To manage complex scenarios such as template literals and string interpolation, the lexer may need to maintain state across multiple characters or lines. This allows it to handle nested structures and ensure proper tokenization even when encountering special constructs.

4. **Preprocessor Support**: The lexer includes support for C-style preprocessor directives (`#define`). This enables the compiler to expand macros before proceeding with the main compilation process, simplifying the codebase and reducing complexity.

5. **Performance Optimization**: Lexical analysis can be computationally intensive, especially for large codebases. Optimizations such as lookahead and caching can significantly improve performance without compromising accuracy.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: `explicit Lexer(const std::string &source);`
  - Initializes the lexer with the source code to be analyzed.

- **tokenize() Function**:
  - Converts the entire source code into a vector of tokens.
  - Returns the vector of tokens representing the source code.

### Private Member Functions

- **current() const**: Retrieves the current character being analyzed.
- **peek(int offset = 1) const**: Looks ahead by a specified number of characters without advancing the position.
- **advance()**: Advances the position to the next character and returns it.
- **skipWhitespace()**: Skips any whitespace characters until a non-whitespace character is encountered.
- **skipComment()**: Skips a single-line comment starting with `//`.
- **skipBlockComment()**: Skips a multi-line comment enclosed within `/* */`.

- **readNumber()**: Reads a numeric literal from the source code and returns it as a `Token`.
- **readString(char quote)**: Reads a string literal enclosed within a specified type of quotes and returns it as a `Token`.
- **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles template literals, expanding them into a sequence of tokens.
- **defines_**: A map storing macro definitions, allowing for easy expansion during preprocessing.
- **pendingTokens_**: Used for managing intermediate tokens during f-string expansion.

## Tradeoffs

1. **Complexity vs. Accuracy**: While supporting advanced features like template literals and macros adds complexity to the lexer, these features enhance the expressiveness and utility of the language. Balancing simplicity and functionality is essential for maintaining a robust and user-friendly compiler.

2. **Performance vs. Memory Usage**: Optimizing the lexer for performance often requires additional memory usage for caching and state management. Finding the right balance between these factors is crucial for maintaining efficiency while accommodating larger codebases.

3. **Readability vs. Maintainability**: Adding more features and optimizations can sometimes reduce the readability of the lexer's implementation. Ensuring that the code remains clean and maintainable is important for long-term development and debugging.

By carefully considering these tradeoffs, the QuantumLanguage compiler aims to deliver a powerful and efficient tool for developers working with quantum programming languages.