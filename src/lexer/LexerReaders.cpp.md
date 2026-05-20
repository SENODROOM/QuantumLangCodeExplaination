# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file is integral to the Quantum Language compiler's lexical analysis phase. Its primary function is to parse template literals, allowing dynamic content within string literals using `${}` syntax. This feature significantly enhances the flexibility of string manipulation and direct embedding of expressions, making it essential for advanced programming constructs.

## Key Design Decisions and Why

### Handling Escaped Characters

To correctly interpret escaped characters within template literals, such as `\n`, `\t`, and ``\` ``$, the lexer uses a state machine approach. When an escape character (`\`) is encountered, the lexer checks the subsequent character to determine its intended meaning. If the subsequent character is not recognized as an escape sequence, the lexer simply appends both characters to the current segment. This ensures that all valid escape sequences are handled accurately while preserving any unrecognized escape sequences.

### Parsing Expressions Within Template Literals

Expressions embedded within template literals are identified by the presence of `${` followed by `}`. To handle these expressions, the lexer maintains a depth counter that increments when encountering `{` and decrements when encountering `}`. This allows the lexer to accurately identify the boundaries of the expression and collect its source code. Once an expression is collected, it is treated separately from the surrounding text, ensuring that it can be re-lexed and parsed independently.

### Removing Empty Parts

To maintain a clean and efficient token sequence, the lexer removes any empty text-only parts that appear at the beginning or end of the template literal. This helps to simplify the resulting token stream and avoid unnecessary processing during later stages of the compilation pipeline.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class contains the main logic for reading and parsing the input source code. The `readTemplateLiteral` function is a member of this class and is responsible for handling the parsing of template literals.

### readTemplateLiteral Function

This function takes two parameters: a reference to a vector of `Token` objects where the parsed tokens will be stored, and the starting line and column numbers for the template literal. The function processes the input source code to extract text segments and embedded expressions, building a list of parts that are then converted into tokens.

## Tradeoffs

### Complexity vs. Flexibility

While the lexer provides robust support for template literals, including handling of escaped characters and embedded expressions, this complexity adds overhead to the lexical analysis process. However, the enhanced flexibility of template literals outweighs this tradeoff, as they enable more powerful and expressive string manipulation capabilities.

### Memory Usage vs. Performance

The lexer stores intermediate parts of template literals in a vector, which may lead to increased memory usage compared to simpler approaches. However, this additional memory usage is generally negligible and provides better performance through improved tokenization and re-lexing of embedded expressions.