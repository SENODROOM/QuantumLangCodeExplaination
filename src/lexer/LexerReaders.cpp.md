# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file is integral to the Quantum Language compiler's lexical analysis phase. Its primary function is to parse template literals, enabling dynamic content within string literals using `${}` syntax. This feature is crucial for languages that require flexible string manipulation and embedding expressions directly within them.

## Key Design Decisions and Why

1. **Efficient Parsing**: The lexer uses a state machine approach to efficiently parse through the input source code. It alternates between collecting text segments and embedded expressions, ensuring minimal overhead during parsing.

2. **Handling Escapes**: Special characters such as `\n`, `\t`, `` ` ``, `\\`, and `$` are handled explicitly to ensure correct interpretation of escape sequences within the template literal.

3. **Expression Nesting**: To support nested expressions within template literals, the lexer maintains a depth counter (`depth`) when encountering `{`. This allows it to correctly identify the end of an expression block.

4. **Tokenization Strategy**: After parsing, the lexer constructs a sequence of tokens based on the collected parts. Text segments are converted into `STRING` tokens, while embedded expressions are re-lexed and wrapped in parentheses to maintain their structure during further processing.

## Major Classes/Functions Overview

### Lexer Class

- **Functionality**: Manages the overall lexing process, including reading different types of literals and handling special characters.
- **Key Methods**:
  - `readTemplateLiteral`: Parses template literals and generates corresponding tokens.
  - `advance`: Moves the lexer forward one character in the input source code.
  - `current`: Returns the current character being processed.

### Part Struct

- **Purpose**: Represents an alternating segment of either plain text or an embedded expression within a template literal.
- **Fields**:
  - `isExpr`: A boolean indicating whether the segment is an embedded expression.
  - `content`: The actual text or expression content.

### readTemplateLiteral Function

- **Parameters**:
  - `std::vector<Token> &out`: The vector where parsed tokens will be stored.
  - `int startLine`: The starting line number of the template literal.
  - `int startCol`: The starting column number of the template literal.
- **Process**:
  - Skips the opening backtick.
  - Alternates between collecting text segments and embedded expressions.
  - Handles escape sequences and nested expressions.
  - Emits `STRING` tokens for text segments and re-lexes embedded expressions within parentheses.
  - Removes any leading or trailing empty text-only parts to keep the output clean.

## Tradeoffs

- **Complexity**: While providing powerful features like embedded expressions, the implementation adds complexity to the lexer. Handling escape sequences and nested expressions requires careful state management.
- **Performance**: The efficient parsing strategy minimizes performance overhead, but more complex scenarios might introduce additional latency.
- **Flexibility vs. Simplicity**: Supporting template literals enhances the language's flexibility, but it also increases the lexer's complexity and potential for errors.

Overall, the `LexerReaders.cpp` file is a vital component of the Quantum Language compiler, enabling its advanced string manipulation capabilities while maintaining a balance between complexity and performance.