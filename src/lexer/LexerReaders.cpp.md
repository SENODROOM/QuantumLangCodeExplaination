# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file plays a critical role in the Quantum Language compiler's lexical analysis phase. It specifically handles the parsing of template literals, which allow dynamic content within string literals using `${}` syntax. This feature enhances flexibility in string manipulation and direct embedding of expressions, making it essential for languages requiring such capabilities.

## Key Design Decisions and Why

### Handling Escaped Characters

To support escaped characters like `\n`, `\t`, etc., within template literals, the lexer uses a state machine approach where it checks for escape sequences (`\`). When an escape character is encountered, it reads the next character and maps it to its corresponding ASCII value or literal representation.

**Why:** This allows users to include special characters directly in their strings without needing to manually handle them as escape codes.

### Nested Expressions

The lexer supports nested expressions within template literals, allowing complex logic to be embedded directly into string values. This is achieved by tracking the depth of curly braces `{}` to ensure balanced parsing of the expression source.

**Why:** Nested expressions provide powerful functionality for generating dynamic strings based on runtime data, enhancing the language's expressiveness and utility.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class contains methods for reading different types of tokens, including template literals. The `readTemplateLiteral` method is central to this functionality, handling the parsing of template literals and converting them into a sequence of tokens.

### readTemplateLiteral Function

This function processes the input source code to identify and parse template literals. It collects alternating text segments and expression sources, building a list of parts. Each part is either a text segment or an expression source enclosed in parentheses.

**Parameters:**
- `std::vector<Token> &out`: A reference to the output vector where parsed tokens will be stored.
- `int startLine`: The starting line number of the template literal.
- `int startCol`: The starting column number of the template literal.

**Process:**
1. **Skip Opening Backtick:** Advances past the initial backtick character that starts the template literal.
2. **Collect Parts:** Iterates through the source code, collecting text segments and expression sources. Handles escape sequences and tracks the depth of curly braces for nested expressions.
3. **Flush Final Segment:** Adds any remaining text segment to the parts list after exiting the loop.
4. **Emit Tokens:** Converts the collected parts into tokens. Text segments become `STRING` tokens, while expression sources are re-lexed and wrapped in parentheses.

## Tradeoffs

### Complexity vs. Usability

While supporting nested expressions adds significant complexity to the lexer, it greatly enhances the usability of template literals by allowing dynamic content generation directly within strings. This balance between complexity and functionality is crucial for maintaining a practical and powerful language.

### Performance vs. Flexibility

Handling escape sequences and nested expressions requires additional processing steps, potentially impacting performance. However, the benefits of these features often outweigh the performance cost, especially in scenarios where dynamic string manipulation is frequent.

### Code Readability vs. Feature Implementation

Implementing advanced features like nested expressions can make the lexer code more complex and harder to understand. Balancing feature implementation with code readability ensures maintainable and scalable codebase.

In conclusion, `LexerReaders.cpp` is a vital component of the Quantum Language compiler, providing essential functionality for template literals. Through careful design decisions and efficient implementation, it strikes a balance between complexity, performance, and usability, ultimately enhancing the language's expressive power and developer experience.