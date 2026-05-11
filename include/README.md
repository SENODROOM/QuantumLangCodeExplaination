# QuantumLanguage Compiler - Token.h

## Overview

The `include/Token.h` header file is an essential part of the QuantumLanguage compiler, responsible for defining tokens used during the lexical analysis phase. Tokens represent the smallest units of meaningful elements in the source code, such as keywords, identifiers, literals, operators, and delimiters. The `Token` struct encapsulates these elements along with their position in the source code.

## Role in Compiler Pipeline

The `Token.h` file plays a pivotal role in the QuantumLanguage compiler's pipeline. During lexical analysis, the input source code is broken down into individual tokens. These tokens are then passed to the parser, which constructs the abstract syntax tree (AST). The AST represents the syntactic structure of the source code, enabling further semantic analysis and code generation stages.

### Key Design Decisions and Why

1. **TokenType Enum**: The `TokenType` enum defines all possible token types. This includes literals (numbers, strings, booleans, nil), identifiers and keywords, operators, delimiters, special tokens like `EOF_TOKEN`, `UNKNOWN`, `INDENT`, and `DEDENT`, and C/C++-style type keywords. Each token type has a specific purpose and helps in distinguishing different parts of the source code.

2. **Token Struct**: The `Token` struct stores information about each token, including its type, value, line number, and column number. This ensures that the compiler can accurately track where each token appears in the source code, facilitating error reporting and debugging.

3. **Position Tracking**: By storing the line and column numbers, the compiler can provide precise error messages when issues arise. This is particularly useful in languages like Python, which rely heavily on indentation for syntax.

4. **String Representation**: The `toString()` method provides a human-readable string representation of the token. This is useful for debugging and logging purposes, allowing developers to easily understand the state of the lexer at any given point.

## Major Classes/Functions Overview

### TokenType Enum

- **Purpose**: Defines all possible token types.
- **Key Types**:
  - Literals (`NUMBER`, `STRING`, `TEMPLATE_STRING`, `BOOL_TRUE`, `BOOL_FALSE`, `NIL`)
  - Identifiers & Keywords (`IDENTIFIER`, `LET`, `CONST`, `FN`, `DEF`, `FUNCTION`, `CLASS`, `EXTENDS`, `NEW`, `THIS`, `SUPER`, `RETURN`, `IF`, `ELSE`, `ELIF`, `WHILE`, `FOR`, `IN`, `OF`, `BREAK`, `CONTINUE`, `RAISE`, `TRY`, `EXCEPT`, `FINALLY`, `AS`, `PRINT`, `INPUT`, `COUT`, `CIN`, `FROM`, `IMPORT`)
  - C/C++ Style Type Keywords (`TYPE_INT`, `TYPE_FLOAT`, `TYPE_DOUBLE`, `TYPE_CHAR`, `TYPE_STRING`, `TYPE_BOOL`, `TYPE_VOID`, `TYPE_LONG`, `TYPE_SHORT`, `TYPE_UNSIGNED`)
  - Operators (`PLUS`, `MINUS`, `STAR`, `SLASH`, `FLOOR_DIV`, `PERCENT`, `POWER`, `EQ`, `NEQ`, `STRICT_EQ`, `STRICT_NEQ`, `NULL_COALESCE`, `LT`, `GT`, `LTE`, `GTE`, `AND`, `OR`, `NOT`, `IS`, `ASSIGN`, `PLUS_ASSIGN`, `MINUS_ASSIGN`, `STAR_ASSIGN`, `SLASH_ASSIGN`, `AND_ASSIGN`, `OR_ASSIGN`, `XOR_ASSIGN`, `MOD_ASSIGN`, `FAT_ARROW`, `PLUS_PLUS`, `MINUS_MINUS`, `BIT_AND`, `BIT_OR`, `BIT_XOR`, `BIT_NOT`, `LSHIFT`, `RSHIFT`, `AND_AND`, `OR_OR`)
  - Delimiters (`LPAREN`, `RPAREN`, `LBRACE`, `RBRACE`, `LBRACKET`, `RBRACKET`, `COMMA`, `SEMICOLON`, `COLON`, `DOT`, `ARROW`, `QUESTION`, `DECORATOR`, `NEWLINE`)
  - Special Tokens (`EOF_TOKEN`, `UNKNOWN`, `INDENT`, `DEDENT`)

### Token Struct

- **Purpose**: Represents a single token in the source code.
- **Attributes**:
  - `type`: The type of the token.
  - `value`: The actual value of the token (e.g., "hello", "42").
  - `line`: The line number where the token starts.
  - `col`: The column number where the token starts.
- **Constructor**: Initializes a `Token` object with the specified type, value, line, and column.
- **Methods**:
  - `toString()`: Returns a string representation of the token, useful for debugging and logging.

## Tradeoffs

1. **Flexibility vs. Simplicity**: While the `TokenType` enum provides a comprehensive list of token types, it may be challenging to maintain and extend over time. A more flexible approach might involve using a dynamic system to define token types, but this could complicate the implementation and increase overhead.

2. **Memory Usage**: Storing the line and column numbers for each token increases memory usage. However, this information is crucial for error reporting and debugging, making the tradeoff worthwhile.

3. **Performance**: The `toString()` method adds some performance overhead, especially in high-performance scenarios. However, this method is primarily used for debugging and logging, so the impact is minimal compared to other critical operations.

By carefully balancing these considerations, the `Token.h` file enables the QuantumLanguage compiler to perform efficient and accurate lexical analysis, setting the foundation for subsequent stages of compilation.