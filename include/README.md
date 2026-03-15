# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential component of the QuantumLanguage compiler, responsible for converting raw text into a sequence of tokens. These tokens serve as the building blocks for further parsing and semantic analysis. The Lexer class plays a pivotal role in the initial stages of the compilation process, ensuring that the input source code is accurately parsed into meaningful units.

## Key Design Decisions

### Use of `std::unordered_map` for Keywords

**WHY**: Using an unordered map allows for constant time complexity (`O(1)`) lookups when checking if a word is a keyword. This is crucial for quickly identifying reserved words during the lexing phase, enhancing performance and reducing overhead.

### Separate Handling of Template Literals

**WHY**: Template literals, which allow for string interpolation using expressions enclosed within `${}`, require special handling to ensure correct tokenization and evaluation. Separating their processing into a dedicated function (`readTemplateLiteral`) ensures clarity and modularity, making it easier to manage and extend the lexer's functionality in the future.

### Use of Pending Tokens for F-Strings

**WHY**: F-strings (formatted string literals) in QuantumLanguage support dynamic content insertion within strings. To handle this, the lexer uses a temporary storage mechanism (`pendingTokens_`) to accumulate tokens before they are finalized. This approach simplifies the integration of f-string expansion logic without cluttering the main lexing process.

## Class and Function Documentation

### Lexer Class

**Purpose**: The `Lexer` class is designed to take a source code string and convert it into a vector of `Token` objects. Each token represents a syntactic element of the source code, such as identifiers, numbers, operators, and keywords.

**Behavior**:
- **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the provided source code string.
- **Function (`std::vector<Token> tokenize()`)**: Processes the entire source code and returns a vector of tokens.

### Private Member Variables

- **src**: Stores the source code string being processed.
- **pos**: Tracks the current position in the source code.
- **line** and **col**: Maintain the current line and column numbers for error reporting and debugging purposes.
- **keywords**: A static unordered map containing all recognized keywords and their corresponding token types.
- **pendingTokens_**: Used temporarily to store tokens while expanding f-strings.
- **defines_**: Stores macro definitions, mapping macro names to lists of replacement tokens.

### Private Functions

- **current() const**: Returns the character at the current position in the source code.
- **peek(int offset = 1) const**: Returns the character at the specified offset ahead of the current position without advancing the position.
- **advance()**: Advances the current position in the source code by one character and returns the character that was previously at the current position.
- **skipWhitespace()**: Skips over any whitespace characters in the source code.
- **skipComment()**: Skips over single-line comments starting with `//`.
- **skipBlockComment()**: Skips over multi-line comments enclosed between `/*` and `*/`.
- **readNumber()**: Reads a numeric literal from the source code and returns the corresponding `Token`.
- **readString(char quote)**: Reads a string literal from the source code, handling both single (`'`) and double (`"`) quotes, and returns the corresponding `Token`.
- **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles the expansion of template literals, accumulating tokens in the provided output vector.
- **readIdentifierOrKeyword()**: Reads an identifier or keyword from the source code and returns the corresponding `Token`.
- **readOperator()**: Reads an operator or punctuation mark from the source code and returns the corresponding `Token`.

## Tradeoffs and Limitations

- **Performance**: While the use of an unordered map for keywords provides fast lookup times, it may introduce additional memory overhead compared to other data structures.
- **Complexity**: Handling template literals requires additional complexity in the lexer, potentially complicating the overall architecture of the compiler.
- **Error Reporting**: The lexer maintains line and column information, which aids in accurate error reporting but adds some computational cost.

This README.md provides a detailed overview of the `Lexer.h` file, explaining its purpose, key design decisions, and the behavior of its major components. It also highlights potential tradeoffs and limitations associated with the implementation.