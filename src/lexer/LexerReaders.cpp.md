# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file is a crucial component of the Quantum Language compiler's lexer stage. It handles the parsing of template literals, which are strings that can contain embedded expressions enclosed in `${}`. This functionality is essential for supporting dynamic content within string literals, a feature commonly found in modern programming languages like JavaScript.

## Key Design Decisions and Why

### Handling Escaped Characters

One of the primary challenges in parsing template literals is handling escaped characters. The code uses a simple state machine where it checks for escape sequences (`\n`, `\t`, `` ` ``, `\\`, `$`). For each recognized escape sequence, it appends the corresponding character to the current segment (`seg`) or handles it appropriately (e.g., appending a backslash followed by an unrecognized character).

**Why:** This approach ensures that all valid escape sequences are correctly interpreted, while unrecognized escape sequences are treated as literal characters, preventing syntax errors.

### Parsing Embedded Expressions

Embedded expressions within template literals are parsed using a nested loop. The outer loop continues until it encounters a closing backtick (```), while the inner loop collects characters until it finds a matching closing brace (`}`). A depth counter is used to handle nested braces within the expression.

**Why:** This method allows for accurate parsing of complex embedded expressions without prematurely terminating the literal. It also supports nesting, which is a common requirement in many programming languages.

### Tokenization of Parts

After collecting all parts of the template literal (text segments and embedded expressions), the code processes them to create a sequence of tokens. Text parts become `STRING` tokens, while embedded expressions are re-lexed and placed within parentheses.

**Why:** This tokenization strategy maintains the integrity of both static and dynamic content within the same string literal. It simplifies subsequent stages of the compilation process by separating these components into distinct types of tokens.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class contains the main logic for reading and parsing the input source code. It includes methods for advancing through the source code, checking the current character, and emitting tokens.

### readTemplateLiteral Function

This function specifically handles the parsing of template literals. It skips the opening backtick, collects alternating text segments and embedded expressions, and then processes these parts to generate a sequence of tokens.

## Tradeoffs

### Complexity vs. Flexibility

While the `readTemplateLiteral` function provides robust support for template literals with features like escaping and nested expressions, it does add complexity to the lexer. This complexity must be managed carefully to ensure efficient parsing and minimal impact on overall performance.

### Memory Usage

Storing the collected parts of the template literal in memory requires additional space. However, this tradeoff is necessary for accurately representing the structure of the literal, especially when dealing with deeply nested expressions.

### Performance Impact

Re-lexing embedded expressions can have a performance impact, particularly if the expressions are large or complex. However, this is often outweighed by the benefits of maintaining dynamic content within string literals.

Overall, the `LexerReaders.cpp` file plays a vital role in the Quantum Language compiler's ability to parse and handle template literals efficiently and flexibly. Its design decisions balance the need for robust parsing capabilities with performance considerations and memory usage.