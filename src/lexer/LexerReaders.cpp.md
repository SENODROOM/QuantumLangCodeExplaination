# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file is integral to the lexical analysis phase of the Quantum Language compiler. Its primary function is to parse template literals, enabling dynamic content within string literals through `${}` syntax. This feature significantly improves the flexibility of string manipulation and the ability to directly embed expressions, thereby enhancing the language's usability and expressiveness.

## Key Design Decisions and Why

1. **Handling Escapes**: The lexer must correctly handle escape sequences such as `\n`, `\t`, `` ` `` (backtick), and others. To achieve this, the code uses a simple state machine that checks for backslashes followed by specific characters and replaces them with their corresponding escape values.

2. **Expression Embedding**: Template literals can contain embedded expressions enclosed in `${}`. The lexer identifies these expressions by looking for the `$` character followed immediately by an opening `{`. Once an expression is identified, it advances past the `{` and collects all characters until a matching `}` is found, effectively isolating the expression source.

3. **Tokenization Strategy**: For text segments, the lexer emits `STRING` tokens. However, for expression segments, instead of emitting them directly as strings, the lexer re-lexes the expression source and wraps the resulting tokens in parentheses. This ensures that the expression is parsed according to its own rules rather than being treated as a single string literal.

4. **Efficiency and Readability**: By breaking down the process into manageable parts—collecting text segments and expressions separately—the code remains efficient and easy to understand. This separation also makes it easier to extend or modify the lexer's behavior in the future without impacting other components.

## Major Classes/Functions Overview

### Lexer Class

- **Functionality**: The `Lexer` class handles the overall lexical analysis of the input source code. It includes methods for reading various types of tokens, including template literals.
- **Key Methods**:
  - `readTemplateLiteral`: Parses template literals, collecting both text segments and embedded expressions.

### readTemplateLiteral Function

- **Purpose**: This function specifically parses template literals, handling both text and expression segments.
- **Parameters**:
  - `std::vector<Token> &out`: A reference to the output vector where parsed tokens will be stored.
  - `int startLine`: The starting line number of the template literal.
  - `int startCol`: The starting column number of the template literal.
- **Process**:
  - Skips the opening backtick.
  - Iterates through the source code, collecting text segments and embedded expressions.
  - Handles escape sequences appropriately.
  - Re-lexes expression segments and wraps them in parentheses before adding them to the output vector.

## Tradeoffs

1. **Complexity vs. Flexibility**: While the lexer adds significant complexity due to the need to handle template literals and re-lex expressions, it provides substantial flexibility and power in string manipulation.

2. **Performance**: The additional processing required to identify and re-lex expressions may impact performance, especially for large inputs. However, the benefits in terms of language expressiveness often outweigh the costs.

3. **Maintainability**: The separation of text and expression segments simplifies the codebase, making it easier to maintain and extend. However, this comes at the cost of increased cognitive load when understanding how the lexer works.

Overall, the `LexerReaders.cpp` file is a critical component of the Quantum Language compiler, offering powerful features at the expense of some complexity. Its well-designed approach ensures that the lexer remains efficient, flexible, and maintainable.