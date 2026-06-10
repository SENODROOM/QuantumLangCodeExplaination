# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file is an essential component of the Quantum Language compiler's lexical analysis phase. It specifically deals with the parsing of template literals, allowing for dynamic content within string literals using `${}` syntax. This feature significantly enhances the flexibility of string manipulation and enables the direct embedding of expressions within strings.

## Key Design Decisions and WHY

1. **Handling Backticks**: The function starts by advancing past the opening backtick (`) character, as it is not part of the literal content.
   
2. **Collecting Parts**: The lexer collects parts of the template literal alternately as text segments and expression source strings. Each part is stored in a `struct` called `Part`, which contains a boolean indicating whether the part is an expression and the actual content of the part.

3. **Escaping Characters**: When encountering escape characters (`\`), the lexer processes them appropriately. For example, `\n` becomes a newline character, and `\t` becomes a tab character. If an unrecognized escape sequence is encountered, it is treated as a literal backslash followed by the character.

4. **Expression Parsing**: When encountering the `${` sequence, the lexer recognizes it as the beginning of an embedded expression. It then collects all characters up to the corresponding `}` character, ensuring that nested curly braces are correctly handled.

5. **Token Emission**: After processing all parts of the template literal, the lexer emits tokens based on these parts. Text segments are emitted as `STRING` tokens, while expression segments are re-lexed and emitted within parentheses.

6. **Trade-offs**:
   - **Complexity**: Handling template literals adds complexity to the lexer due to the need to parse both text and expression segments.
   - **Performance**: While the lexer needs to handle more cases, the performance impact is generally minimal compared to other phases of the compiler.
   - **Flexibility**: The ability to embed expressions directly within strings provides significant flexibility but requires careful handling to ensure correct parsing and evaluation.

## Major Classes/Functions Overview

### Class: Lexer
- **Function: readTemplateLiteral**
  - **Parameters**: 
    - `std::vector<Token> &out`: A reference to the output vector where parsed tokens will be stored.
    - `int startLine`: The starting line number of the literal.
    - `int startCol`: The starting column number of the literal.
  - **Purpose**: Parses a template literal and populates the output vector with the appropriate tokens.
  - **Logic**:
    - Skips the opening backtick.
    - Collects alternating text segments and expression sources.
    - Handles escaping special characters.
    - Recognizes and parses embedded expressions.
    - Emits `STRING` tokens for text segments and re-lexed tokens within parentheses for expression segments.

### Struct: Part
- **Members**:
  - `bool isExpr`: Indicates whether the part is an expression.
  - `std::string content`: Holds the content of the part.
- **Purpose**: Stores information about each segment collected during the parsing of the template literal.

## Tradeoffs

- **Increased Complexity**: The lexer must now handle multiple types of segments (text and expressions), which increases its complexity.
- **Potential Performance Overhead**: Although minimal, there might be a slight performance overhead due to the additional logic required for parsing and handling template literals.
- **Enhanced Flexibility**: The ability to embed expressions directly within strings offers significant flexibility but requires careful implementation to avoid errors.

Overall, the `LexerReaders.cpp` file is a critical piece of the Quantum Language compiler, enabling powerful string manipulation capabilities through template literals. Its design balances increased complexity with enhanced functionality, providing a robust solution for parsing these literals.