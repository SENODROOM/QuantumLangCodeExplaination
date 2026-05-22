# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file plays a crucial role in the lexical analysis phase of the Quantum Language compiler. It is responsible for parsing template literals, which allow dynamic content within string literals using `${}` syntax. This feature greatly enhances the flexibility of string manipulation and direct embedding of expressions, making it an essential component of the compiler.

## Key Design Decisions and Why

### Handling Escaped Characters
The lexer must correctly handle escaped characters such as `\n`, `\t`, `` ` `` (backtick), and others. To achieve this, the code uses a simple state machine where it checks for escape sequences starting with a backslash (`\`). If a recognized escape sequence is found, it replaces the escape character with its corresponding ASCII value or literal. For unrecognized escape sequences, the lexer emits a warning and leaves the original escape character unchanged.

### Parsing Expressions Within Template Literals
Expressions embedded within template literals are enclosed in `${}`. The lexer identifies these expressions by looking for the dollar sign followed immediately by a curly brace (`$}`). Once an expression is identified, it advances past the curly brace and collects all characters until another closing curly brace is encountered, indicating the end of the expression. This allows the lexer to treat the expression as a separate token that can be re-lexed later during the parsing phase.

### Trade-offs Considered

#### Complexity vs. Flexibility
While handling complex escape sequences adds some complexity to the lexer, it provides significant flexibility in string manipulation. The decision was made to support common escape sequences directly rather than implementing a full-fledged string parser.

#### Performance vs. Robustness
To improve performance, the lexer avoids unnecessary re-lexing of expressions. Instead, it treats them as separate tokens that can be processed by subsequent stages of the compiler. This approach balances robustness, as it ensures correct parsing of both text and expressions, with performance, as it minimizes redundant processing.

## Major Classes/Functions Overview

### Lexer Class
The `Lexer` class contains the main logic for reading and lexing the input source code. It includes methods like `readTemplateLiteral` for handling template literals specifically.

### readTemplateLiteral Function
This function is central to the lexer's functionality for template literals. It processes the input stream, collecting alternating text segments and expression sources. Each text segment is treated as a `STRING` token, while each expression source is temporarily stored as a raw string and will be re-lexed later.

### Part Struct
A helper structure named `Part` is used to store information about each segment collected during the parsing process. Each `Part` object indicates whether the segment is an expression or plain text and stores the actual content of the segment.

### emitPlus Lambda Function
This lambda function is used to insert a `PLUS` token between different parts of the template literal when building the output token sequence. It helps maintain proper syntax tree construction by ensuring that text segments are concatenated appropriately.

## Conclusion

The `LexerReaders.cpp` file is a vital part of the Quantum Language compiler's lexical analysis phase, enabling the efficient parsing of template literals. By carefully handling escape sequences and identifying expressions within template literals, the lexer ensures that the compiler can accurately process and generate the appropriate abstract syntax trees. The design choices balance complexity and flexibility, performance and robustness, resulting in a reliable and efficient implementation of template literal parsing.