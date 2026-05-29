# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential part of the QuantumLanguage compiler, primarily responsible for converting source code into tokens. These tokens serve as the fundamental building blocks for further processing stages such as parsing and semantic analysis. The lexer plays a crucial role in accurately interpreting the syntax of the language, which is vital for generating correct intermediate representations and executable code.

## Role in Compiler Pipeline

In the QuantumLanguage compiler pipeline, the lexer operates at the beginning stage. It reads the input source code character by character and categorizes them into meaningful tokens based on the language's grammar rules. These tokens are then passed to the parser, which constructs the abstract syntax tree (AST). Finally, the AST is analyzed semantically, and the compiler generates the target code.

## Key Design Decisions and Why

### Tokenization Strategy

The lexer uses a state-based approach to tokenize the input source code. This method allows it to efficiently handle different types of tokens, including numbers, strings, identifiers, operators, and special characters like whitespace and comments. By breaking down the source code into discrete tokens, the lexer simplifies the subsequent stages of compilation.

### Handling Keywords and Identifiers

To distinguish between keywords and identifiers, the lexer maintains a lookup table (`keywords`) that contains all valid keyword strings along with their corresponding token types. When encountering an identifier, the lexer checks this table to determine whether it should be classified as a keyword or a regular identifier. This ensures that the language's reserved words are correctly recognized and handled during the compilation process.

### Support for Template Literals

One of the unique features of the QuantumLanguage compiler is its support for template literals, similar to Python's f-strings. To accommodate this feature, the lexer includes specialized functions (`readTemplateLiteral`) that can handle multi-line string literals containing embedded expressions. This capability enhances the flexibility and expressiveness of the language, making it easier for developers to write complex and readable code.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class encapsulates the logic for tokenizing the input source code. It provides the following functionalities:

- **Constructor**: Initializes the lexer with the source code string.
- **tokenize() Method**: Reads through the source code and returns a vector of tokens representing the parsed elements.

### Private Member Functions

The `Lexer` class also includes several private member functions that assist in the tokenization process:

- **current()**: Returns the current character being processed.
- **peek(int offset)**: Looks ahead in the source code without advancing the position.
- **advance()**: Advances the lexer's position to the next character.
- **skipWhitespace()**: Skips over any whitespace characters encountered.
- **skipComment()**: Skips over single-line comments starting with `//`.
- **skipBlockComment()**: Skips over multi-line comments enclosed within `/* */`.
- **readNumber()**: Parses numeric literals and returns a token.
- **readString(char quote)**: Parses string literals enclosed by specified quotes and returns a token.
- **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles template literal parsing, expanding embedded expressions and returning a vector of tokens.
- **readIdentifierOrKeyword()**: Parses identifiers and checks against the keyword table to return the appropriate token type.
- **readOperator()**: Parses operator symbols and returns a token.

## Tradeoffs

While the lexer provides robust functionality for tokenizing the source code, there are some tradeoffs to consider:

- **Complexity**: Implementing a comprehensive lexer requires careful consideration of the language's grammar rules, leading to increased complexity in the codebase.
- **Performance**: Efficiently processing large amounts of source code while maintaining accuracy can pose performance challenges. Optimizations may be necessary to ensure fast and reliable tokenization.
- **Flexibility**: Supporting advanced features like template literals adds flexibility but may complicate the lexer's implementation and testing.

Overall, the `include/Lexer.h` header file serves as a critical foundation for the QuantumLanguage compiler, enabling accurate and efficient tokenization of the source code. Its design decisions and features contribute to the language's expressive power and ease of use.