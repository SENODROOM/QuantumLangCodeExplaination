# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential component of the QuantumLanguage compiler, focusing on the lexical analysis phase. This phase involves breaking down the source code into meaningful tokens, which are then processed by subsequent phases of the compiler. The `Lexer` class is designed to handle the conversion of raw text into a sequence of tokens, taking care of various aspects such as whitespace, comments, numbers, strings, identifiers, and operators.

## Role in Compiler Pipeline

In the context of the QuantumLanguage compiler's pipeline, the `Lexer` class operates at the beginning, converting the input source code into a stream of tokens. These tokens serve as the foundation for further stages like parsing and semantic analysis. By accurately identifying and categorizing different elements of the source code, the lexer facilitates efficient and error-free compilation.

## Key Design Decisions and Why

1. **Tokenization with State Management**: The `Lexer` class uses state management to track its position within the source code. This allows it to handle complex cases like template literals and string interpolation effectively without getting confused by nested structures or boundary conditions.

2. **Support for Preprocessor Macros**: The inclusion of a mechanism to handle C-style preprocessor macros (`#define`) adds flexibility to the language. This feature enables developers to define constants, functions, or other constructs that can be expanded during the preprocessing stage, enhancing code reusability and maintainability.

3. **Handling Whitespace and Comments**: Efficiently skipping over whitespace and comments is critical for performance and readability. The lexer provides separate methods (`skipWhitespace`, `skipComment`, `skipBlockComment`) to handle these scenarios, ensuring that they do not interfere with the tokenization process.

4. **Error Handling**: While not explicitly shown in the provided code snippet, the lexer should have robust error handling mechanisms to manage unexpected characters or malformed tokens gracefully. This helps in providing clear feedback to the developer about issues in their source code.

## Major Classes/Functions Overview

- **Lexer Class**:
  - **Constructor**: Initializes the lexer with the source code.
  - **tokenize() Method**: Main method that processes the entire source code and returns a vector of tokens.

- **Private Methods**:
  - **current()**: Returns the character at the current position.
  - **peek(int offset)**: Returns the character at the specified offset ahead of the current position.
  - **advance()**: Advances the lexer's position by one character and returns the character that was skipped.
  - **skipWhitespace()**: Skips all whitespace characters until a non-whitespace character is encountered.
  - **skipComment()**: Skips a single-line comment starting with `//`.
  - **skipBlockComment()**: Skips a multi-line comment enclosed between `/*` and `*/`.
  - **readNumber()**: Reads and returns a numeric token from the source code.
  - **readString(char quote)**: Reads and returns a string token from the source code, handling both single and double quotes.
  - **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles the reading of template literals, expanding them into a sequence of tokens.
  - **readIdentifierOrKeyword()**: Reads and returns either an identifier or a keyword from the source code.
  - **readOperator()**: Reads and returns an operator token from the source code.

## Tradeoffs

- **Complexity vs. Performance**: Managing the state and handling edge cases like template literals increases the complexity of the lexer but improves its ability to correctly parse the source code. This tradeoff is necessary for accurate and efficient compilation.
  
- **Flexibility vs. Simplicity**: Supporting preprocessor macros adds flexibility but also complicates the lexer's implementation. Balancing these features requires careful consideration of how often macros are used and the potential impact on performance.

- **Readability vs. Functionality**: Splitting the functionality into multiple private methods enhances readability and modularity. However, this might introduce some overhead due to function calls, especially if the lexer is invoked frequently.

Overall, the `Lexer.h` file is a vital part of the QuantumLanguage compiler, ensuring that the source code is accurately converted into tokens. Its design decisions reflect a balance between functionality, performance, and simplicity, making it a robust tool for the lexical analysis phase.