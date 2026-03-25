# QuantumLanguage Compiler - Token.h

## Overview

The `include/Token.h` header file is a fundamental component of the QuantumLanguage compiler, dedicated to defining tokens used during the lexical analysis phase. Tokens represent the smallest units of meaningful elements in the source code, such as keywords, identifiers, literals, and operators. This file provides an enumeration of token types (`TokenType`) and a structure (`Token`) to encapsulate these tokens along with their associated metadata.

## Role in Compiler Pipeline

- **Lexical Analysis**: Converts raw source code into a sequence of tokens.
- **Syntax Parsing**: Uses these tokens to construct an abstract syntax tree (AST).
- **Semantic Analysis**: Validates the AST based on language rules and semantics.
- **Code Generation**: Translates the AST into target machine code or intermediate representations.

## Key Design Decisions and Why

1. **Enum Class for Token Types**:
   - **Why**: Using an enum class ensures type safety and prevents misuse of token types.
   - **Tradeoff**: Slightly more verbose than plain enums but enhances code readability and maintainability.

2. **Token Structure**:
   - **Type**: Stores the type of the token.
   - **Value**: Holds the string representation of the token.
   - **Line and Column**: Keeps track of the position in the source code where the token was found.
   - **Constructor**: Initializes the token with its type, value, and location.
   - **toString Method**: Provides a human-readable string representation of the token, useful for debugging and logging.

3. **String Value Storage**:
   - **Why**: Storing token values as strings allows easy manipulation and comparison.
   - **Tradeoff**: Increases memory usage compared to storing simpler data types like integers.

4. **Position Tracking**:
   - **Why**: Maintaining line and column information helps in error reporting and debugging.
   - **Tradeoff**: Adds complexity to the token structure but improves developer experience.

## Major Classes/Functions Overview

### `TokenType Enum Class`
- **Purpose**: Defines all possible token types used in the QuantumLanguage compiler.
- **Key Members**:
  - Literal types (`NUMBER`, `STRING`, `TEMPLATE_STRING`, etc.)
  - Identifier and keyword types (`IDENTIFIER`, `LET`, `CONST`, etc.)
  - Operator types (`PLUS`, `MINUS`, `STAR`, etc.)
  - Delimiter types (`LPAREN`, `RPAREN`, `LBRACE`, etc.)
  - Special types (`EOF_TOKEN`, `UNKNOWN`, `INDENT`, `DEDENT`)

### `Token Struct`
- **Purpose**: Represents a single token in the source code.
- **Members**:
  - `type`: The type of the token.
  - `value`: The string value of the token.
  - `line`: The line number in the source code where the token appears.
  - `col`: The column number in the source code where the token appears.
- **Methods**:
  - `Token(TokenType t, std::string v, int ln, int c)`: Constructor to initialize the token.
  - `std::string toString() const`: Returns a string representation of the token.

## Tradeoffs

- **Memory Usage**: Storing token values as strings increases memory consumption.
- **Performance**: String operations can be slower compared to primitive data types.
- **Readability**: Enum class and structured token representation improve code readability and maintainability.
- **Complexity**: Additional tracking of positions and special types adds complexity to both the token structure and the lexer implementation.

Overall, the `Token.h` file serves as a cornerstone for the QuantumLanguage compiler's lexical analysis phase, providing a robust and flexible framework for representing and manipulating tokens throughout the compilation process.