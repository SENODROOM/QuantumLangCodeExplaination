# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is integral to the QuantumLanguage compiler, focusing on the lexical analysis phase of the compilation process. This phase involves breaking down the source code into meaningful tokens, which are then processed by subsequent phases such as parsing and semantic analysis.

## Role in Compiler Pipeline

The Lexer class operates at the beginning of the compiler's pipeline. It takes the raw source code as input and converts it into a sequence of tokens. These tokens represent the smallest units of meaning in the language, such as keywords, identifiers, operators, and literals. By converting the source code into tokens, the Lexer facilitates the parsing stage, where these tokens are organized into a structured representation of the program.

## Key Design Decisions and Why

### Tokenization Rules

The Lexer uses predefined rules to identify different types of tokens. For example, it recognizes numbers, strings, identifiers, and operators. This approach ensures that the source code is parsed correctly according to the language's syntax rules.

### Handling Keywords

A static unordered map named `keywords` is used to store reserved words of the language. When the Lexer encounters an identifier, it checks against this map to determine if it should be classified as a keyword or an ordinary identifier. This helps in distinguishing between language constructs and user-defined names.

### Managing Multi-Line Strings

The Lexer supports multi-line string literals using template literals. When encountering a backtick (`), it calls `readTemplateLiteral`, which reads until another backtick is found, allowing for embedded expressions within the string. This feature enhances the flexibility of string manipulation in the language.

### Preprocessor Macros

The Lexer also handles C-style preprocessor macros. It maintains an unordered map called `defines_` to store macro names and their corresponding replacements. This allows for conditional compilation and simplifies code reuse through macro definitions.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the Lexer with the source code as a string.
- **tokenize Method**: Converts the entire source code into a vector of tokens.

### Private Methods

- **current Method**: Returns the character at the current position in the source code.
- **peek Method**: Returns the character at a specified offset ahead without advancing the position.
- **advance Method**: Advances the position in the source code and returns the character at the new position.
- **skipWhitespace Method**: Skips over any whitespace characters in the source code.
- **skipComment Method**: Skips over single-line comments starting with `//`.
- **skipBlockComment Method**: Skips over multi-line comments enclosed within `/* */`.
- **readNumber Method**: Reads a numeric literal from the source code.
- **readString Method**: Reads a string literal from the source code, handling both single and double quotes.
- **readTemplateLiteral Method**: Reads a template literal, including embedded expressions.
- **readIdentifierOrKeyword Method**: Reads an identifier and checks if it is a keyword.
- **readOperator Method**: Reads an operator or punctuation symbol.

## Tradeoffs

### Complexity vs. Flexibility

While the Lexer provides robust support for various token types and features like template literals and preprocessor macros, this complexity can lead to increased maintenance overhead. However, it offers significant flexibility and power in the language, making it easier for developers to write complex and expressive programs.

### Performance vs. Accuracy

Efficiently skipping over whitespace and comments is crucial for performance, but it requires careful handling to avoid misinterpreting valid code. The Lexer strikes a balance by implementing optimized algorithms for these tasks, ensuring both speed and accuracy in tokenization.

### Memory Usage vs. Functionality

Maintaining state information such as the current position, line number, and column number adds to memory usage. However, this is necessary for accurate error reporting and context management during the compilation process. The trade-off here is between additional memory consumption and improved debugging capabilities.

In conclusion, the `Lexer.h` header file is a vital component of the QuantumLanguage compiler, providing essential functionality for breaking down source code into tokens. Its design balances complexity, flexibility, performance, and memory usage to ensure efficient and accurate lexical analysis.