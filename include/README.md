# QuantumLanguage Compiler - Lexer.h

## Overview

The `Lexer.h` header file is an essential part of the QuantumLanguage compiler, responsible for converting source code into tokens. Tokens represent the smallest meaningful units of the language, such as keywords, identifiers, numbers, strings, and operators. The lexer plays a critical role in the compilation process by breaking down the source code into these discrete elements, which are then processed by subsequent stages of the compiler.

## Role in Compiler Pipeline

The Lexer operates at the beginning of the compiler pipeline, taking raw source code as input and producing a sequence of tokens. These tokens serve as the foundation for further parsing and semantic analysis. The primary responsibilities of the Lexer include:

- **Tokenization**: Converting the source code into individual tokens based on predefined rules.
- **Error Handling**: Identifying and reporting syntax errors encountered during tokenization.
- **Context Management**: Maintaining information about the current position in the source code, including line and column numbers, to provide precise error messages.

By accurately tokenizing the source code, the Lexer ensures that the subsequent parser can build a correct abstract syntax tree (AST), leading to more reliable and efficient compilation.

## Key Design Decisions and Why

### Tokenization Rules

The Lexer uses a set of rules to determine how to break down the source code into tokens. These rules are defined using regular expressions and include handling of comments, strings, numbers, and operators. The decision to use regular expressions simplifies the implementation of complex tokenization patterns while maintaining readability and maintainability.

### Error Reporting

To improve the debugging experience, the Lexer includes detailed error reporting capabilities. It not only identifies syntax errors but also provides information about their location in the source code (line and column). This helps developers quickly locate and fix issues in their code.

### Context Information

Maintaining context information such as line and column numbers is crucial for generating accurate error messages. By keeping track of these details, the Lexer ensures that any issues are reported with the most relevant and precise information possible.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class encapsulates the functionality required for tokenization. Its main components include:

- **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the source code.
- **Tokenize Function (`std::vector<Token> tokenize()`)**: Converts the source code into a vector of tokens.

#### Private Members

- **Source Code (`std::string src`)**: Stores the source code being tokenized.
- **Position (`size_t pos`)**: Tracks the current position within the source code.
- **Line and Column Numbers (`int line`, `int col`)**: Maintain information about the current line and column in the source code.
- **Keywords Map (`static const std::unordered_map<std::string, TokenType> keywords`)**: Maps keyword strings to their corresponding token types.
- **Pending Tokens (`std::vector<Token> pendingTokens_`)**: Used for handling f-string expansions.
- **Macros Map (`std::unordered_map<std::string, std::vector<Token>> defines_`)**: Stores macro definitions for C-style preprocessor directives.

#### Private Methods

- **Current Character (`char current() const`)**: Returns the character at the current position.
- **Peek Character (`char peek(int offset = 1) const`)**: Returns the character at the specified offset without advancing the position.
- **Advance Position (`char advance()`)**: Advances the position to the next character and returns it.
- **Skip Whitespace (`void skipWhitespace()`)**: Skips over whitespace characters until a non-whitespace character is found.
- **Skip Comment (`void skipComment()`)**: Skips over single-line comments starting with `//`.
- **Skip Block Comment (`void skipBlockComment()`)**: Skips over multi-line comments enclosed between `/*` and `*/`.

- **Read Number (`Token readNumber()`)**: Reads a number token from the source code.
- **Read String (`Token readString(char quote)`)**: Reads a string token from the source code, handling both single and double quotes.
- **Read Template Literal (`void readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)`)**: Handles template literals, expanding them into multiple tokens.
- **Read Identifier or Keyword (`Token readIdentifierOrKeyword()`)**: Reads an identifier or keyword token from the source code.
- **Read Operator (`Token readOperator()`)**: Reads an operator token from the source code.

These methods work together to efficiently tokenize the source code while handling various edge cases and providing robust error reporting.

## Tradeoffs

While the Lexer provides powerful features for tokenization and error reporting, there are some tradeoffs to consider:

- **Complexity**: Implementing a comprehensive lexer requires careful consideration of all possible token patterns and edge cases. This adds complexity to the implementation and increases the likelihood of bugs.
- **Performance**: The lexer must be efficient enough to handle large source files without significant performance degradation. Optimizations such as lookahead and caching can help mitigate this issue.
- **Flexibility**: Providing extensive support for features like f-string expansions and C-style preprocessor directives increases flexibility but may complicate the lexer's logic.

Despite these challenges, the Lexer remains a vital component of the QuantumLanguage compiler, enabling accurate and efficient tokenization of source code.