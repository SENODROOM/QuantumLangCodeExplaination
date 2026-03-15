# Lexer.h in QuantumLanguage Compiler

## Overview

The `Lexer.h` header file is an integral part of the QuantumLanguage compiler, specifically designed for lexical analysis. This process involves breaking down the input source code into meaningful tokens that can then be processed by the parser. The `Lexer` class plays a pivotal role in this step by converting raw characters into structured tokens, which represent different elements like identifiers, numbers, strings, operators, and keywords.

## Key Design Decisions

### Use of `std::variant` for Token Types

**WHY:** Leveraging `std::variant` allows the lexer to encapsulate multiple potential token types within a single variable, simplifying the handling and management of these diverse types. This decision enhances flexibility and reduces boilerplate code compared to traditional union-based approaches.

### Support for F-Strings

**WHY:** F-strings provide a powerful feature for string formatting in Python-like languages. By implementing support for f-strings in the lexer, we enable the compiler to handle complex string manipulations directly at the lexical level, facilitating more efficient parsing and interpretation.

### Preprocessor Macros

**WHY:** Incorporating a mechanism to parse and expand preprocessor macros (`#define`) is essential for maintaining compatibility with existing C/C++ codebases. This feature allows the lexer to preprocess the source code before further processing, ensuring that any macro expansions are correctly handled during the compilation phase.

## Class and Function Documentation

### Lexer Class

**Purpose:** The `Lexer` class is the primary class responsible for performing lexical analysis on the source code. It initializes with a source string and processes it character by character to generate a sequence of tokens.

**Behavior:**
- **Constructor:** Takes a `const std::string &source` as input and initializes the lexer.
- **tokenize():** Processes the entire source code and returns a vector of `Token` objects representing the lexical structure of the code.

### Private Member Functions

#### current() const

**Purpose:** Returns the current character being processed by the lexer without advancing the position.

#### peek(int offset = 1) const

**Purpose:** Returns the character at a specified offset ahead of the current position without advancing the position.

#### advance()

**Purpose:** Advances the lexer's position to the next character and returns it.

#### skipWhitespace()

**Purpose:** Skips over any whitespace characters in the source code.

#### skipComment()

**Purpose:** Skips over a single-line comment starting with `//`.

#### skipBlockComment()

**Purpose:** Skips over a multi-line comment enclosed between `/*` and `*/`.

#### readNumber()

**Purpose:** Reads and constructs a numeric token from the source code.

#### readString(char quote)

**Purpose:** Reads and constructs a string token from the source code, using the provided quote character.

#### readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)

**Purpose:** Handles the reading and expansion of template literals (f-strings) in the source code, storing the resulting tokens in the provided output vector.

#### readIdentifierOrKeyword()

**Purpose:** Reads and constructs an identifier or keyword token from the source code.

#### readOperator()

**Purpose:** Reads and constructs an operator token from the source code.

## Tradeoffs and Limitations

- **Complexity:** Supporting f-strings and preprocessor macros adds complexity to the lexer, requiring additional logic to handle these features.
- **Performance:** While `std::variant` provides flexibility, it may introduce slight performance overhead compared to simpler data structures.
- **Error Handling:** The lexer primarily focuses on tokenization and may not catch certain syntax errors immediately. Error handling is typically addressed by the parser.

This README.md provides a comprehensive overview of the `Lexer.h` file, detailing its functionality, design decisions, and potential limitations.