# LexerCore Component of the Quantum Language Compiler

The `lexercore` component is a fundamental module in the Quantum Language (QL) compiler responsible for converting source code into a sequence of tokens. These tokens are essential for the parser to generate an abstract syntax tree (AST). This README provides an overview of the primary functionalities and their relationships within the component.

## Overview

The `lexercore` component includes several key functions that work together to tokenize the input source code:

1. **current**: Returns the current token being processed.
2. **peek**: Looks ahead at the next token without advancing the lexer.
3. **advance**: Advances the lexer to the next token.
4. **skipWhitespace**: Skips over any whitespace characters in the source code.
5. **skipComment**: Skips over single-line comments starting with `//`.
6. **skipBlockComment**: Skips over multi-line comments enclosed between `/*` and `*/`.
7. **readNumber**: Reads a numeric literal from the source code.

These functions collaborate to ensure that the lexer accurately identifies and categorizes different elements of the source code, such as keywords, identifiers, literals, and operators.

## File Structure

- **lexer.h**: Contains declarations for all public functions and classes used by the lexer.
- **lexer.cpp**: Implements the functionality declared in `lexer.h`.

## Functionality Details

### current

**Purpose**: Returns the current token being processed.

**Usage**:
```cpp
Token currentToken = lexer.current();
```

### peek

**Purpose**: Looks ahead at the next token without advancing the lexer.

**Usage**:
```cpp
Token nextToken = lexer.peek();
```

### advance

**Purpose**: Advances the lexer to the next token.

**Usage**:
```cpp
lexer.advance();
```

### skipWhitespace

**Purpose**: Skips over any whitespace characters in the source code.

**Usage**:
```cpp
lexer.skipWhitespace();
```

### skipComment

**Purpose**: Skips over single-line comments starting with `//`.

**Usage**:
```cpp
lexer.skipComment();
```

### skipBlockComment

**Purpose**: Skips over multi-line comments enclosed between `/*` and `*/`.

**Usage**:
```cpp
lexer.skipBlockComment();
```

### readNumber

**Purpose**: Reads a numeric literal from the source code.

**Usage**:
```cpp
Token numberToken = lexer.readNumber();
```

## Overall Flow

1. The lexer initializes with the source code.
2. It skips over any leading whitespace using `skipWhitespace()`.
3. If it encounters a comment (`//` or `/* */`), it skips the comment using `skipComment()` or `skipBlockComment()`, respectively.
4. For each character in the source code, the lexer determines its type (e.g., keyword, identifier, literal).
5. If a numeric literal is detected, it reads the entire number using `readNumber()`.
6. The lexer advances to the next token using `advance()` after processing each element.
7. This process continues until the end of the source code is reached.

By following this flow, the `lexercore` component ensures that the source code is accurately converted into a stream of tokens, which can then be used for further parsing and semantic analysis.

For more detailed information on each function, refer to the individual function documentation within `lexer.h`.