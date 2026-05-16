# QuantumLanguage Compiler - Token.h

## Overview

The `include/Token.h` header file is a crucial component of the QuantumLanguage compiler, focusing on the representation and management of tokens within the source code. Tokens serve as the basic units of input processed by the compiler, forming the syntax tree upon which semantic analysis and code generation are based.

## Role in Compiler Pipeline

In the QuantumLanguage compiler's pipeline, `Token.h` plays a pivotal role during the lexical analysis phase. This phase involves converting raw source code into a sequence of tokens, each carrying information about its type, value, and location within the source file. These tokens are then passed to the parser, which constructs the abstract syntax tree (AST). The AST serves as the foundation for further stages of compilation, including semantic analysis, optimization, and code generation.

## Key Design Decisions and Why

1. **TokenType Enum**: The `TokenType` enum defines all possible token types, covering literals, identifiers, keywords, operators, delimiters, special cases like indentation and EOF, and unknown tokens. Each token type is carefully chosen to accurately reflect the syntactic elements of the language.

2. **Token Struct**: The `Token` struct encapsulates the properties of a token, including its type, value, line number, and column position. This design allows for easy tracking and manipulation of tokens throughout the compilation process.

3. **String Value Storage**: Storing token values as strings provides flexibility and ensures that the compiler can handle various data types without requiring additional storage mechanisms.

4. **Location Information**: Including line and column numbers helps in diagnosing errors and providing precise feedback to the user, making debugging easier and more effective.

## Major Classes/Functions Overview

### `Token` Struct

- **Purpose**: Represents a single token in the source code.
- **Properties**:
  - `type`: The type of the token (e.g., `NUMBER`, `IDENTIFIER`).
  - `value`: The string value of the token.
  - `line`: The line number where the token appears in the source code.
  - `col`: The column number where the token starts in the source code.
- **Methods**:
  - `toString() const`: Returns a string representation of the token, useful for debugging and logging purposes.

### `TokenType` Enum

- **Purpose**: Defines all valid token types in the QuantumLanguage.
- **Values**:
  - Literals (`NUMBER`, `STRING`, `TEMPLATE_STRING`, `BOOL_TRUE`, `BOOL_FALSE`, `NIL`).
  - Identifiers and Keywords (`LET`, `CONST`, `FN`, `DEF`, `FUNCTION`, `CLASS`, `EXTENDS`, `NEW`, `THIS`, `SUPER`, `RETURN`, `IF`, `ELSE`, `ELIF`, `WHILE`, `FOR`, `IN`, `OF`, `BREAK`, `CONTINUE`, `RAISE`, `TRY`, `EXCEPT`, `FINALLY`, `AS`, `PRINT`, `INPUT`, `COUT`, `CIN`, `FROM`, `IMPORT`).
  - C/C++ Style Type Keywords (`TYPE_INT`, `TYPE_FLOAT`, etc.).
  - Operators (`PLUS`, `MINUS`, `STAR`, etc.).
  - Delimiters (`LPAREN`, `RPAREN`, etc.).
  - Special Cases (`INDENT`, `DEDENT`, `EOF_TOKEN`, `UNKNOWN`).

## Tradeoffs

- **Flexibility vs. Complexity**: By using a single `std::string` to store token values, the implementation remains simple but may require additional parsing logic for different data types.
- **Memory Usage**: Storing token values as strings can lead to increased memory usage compared to storing them directly in their respective data types.
- **Error Handling**: Providing detailed location information (line and column numbers) enhances error handling capabilities, but adds complexity to token processing.

Overall, the `Token.h` header file is a fundamental building block of the QuantumLanguage compiler, enabling efficient and accurate lexical analysis and serving as the basis for subsequent stages of compilation. Its design choices balance simplicity with functionality, making it well-suited for the needs of a modern programming language compiler.