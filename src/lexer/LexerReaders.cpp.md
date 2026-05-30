# LexerReaders.cpp

## Role in Compiler Pipeline

The `LexerReaders.cpp` file plays a crucial role in the lexical analysis phase of the Quantum Language compiler. Specifically, it handles the parsing of template literals, which allow for dynamic content within string literals using `${}` syntax. This feature enhances the flexibility of string manipulation and enables direct embedding of expressions, making the code more readable and expressive.

## Key Design Decisions and Why

1. **Handling Escapes**: The lexer needs to correctly handle escape sequences within the template literal. For example, `\n`, `\t`, and `` \` `` should be interpreted as newline, tab, and backtick respectively. To achieve this, the lexer checks for backslashes (`\`) and processes them accordingly, ensuring that the correct characters are emitted as tokens.

2. **Expression Parsing**: Template literals can contain embedded expressions enclosed in `${}`. The lexer must identify these expressions and treat them differently from regular text. It does this by looking for the dollar sign (`$`) followed immediately by an open brace (`{`). When such a pattern is detected, it starts collecting characters until a matching close brace (`}`) is found, effectively isolating the expression source string.

3. **Tokenization**: Once the lexer has collected all parts of the template literal—both text segments and expression source strings—it needs to tokenize them appropriately. Regular text segments become `STRING` tokens, while expression source strings are re-lexed and placed within parentheses to maintain their structure during further processing phases of the compiler.

4. **Efficiency**: The lexer aims to efficiently process template literals without introducing unnecessary complexity. By maintaining a simple state machine-like approach, it ensures that each character is processed exactly once, leading to linear time complexity relative to the size of the input string.

## Major Classes/Functions Overview

### Class: Lexer

The `Lexer` class contains the core logic for reading and lexing the source code. It includes methods like `readTemplateLiteral` which specifically targets the parsing of template literals.

#### Function: readTemplateLiteral

This function is responsible for parsing template literals. It takes two parameters: `std::vector<Token> &out` where the resulting tokens will be stored, and `int startLine` and `int startCol` indicating the starting line and column of the template literal in the source code.

- **State Machine**: The function uses a state machine to iterate over the source code. It skips the opening backtick and collects parts of the template literal alternately as text segments or expression source strings.
- **Escape Handling**: Inside the loop, it checks for backslashes and processes escape sequences accordingly.
- **Expression Identification**: When a `$` followed by `{` is encountered, it identifies the start of an expression and collects characters until a matching `}` is found.
- **Token Emission**: After collecting all parts, it emits tokens based on whether each part is a text segment or an expression. Regular text segments become `STRING` tokens, while expressions are re-lexed and placed within parentheses.

## Tradeoffs

1. **Complexity vs. Flexibility**: While the lexer introduces additional complexity to handle template literals, it provides significant benefits in terms of code readability and expressiveness. The tradeoff is that developers need to be aware of the `${}` syntax when writing template literals.

2. **Performance**: The lexer's efficiency is maintained through a straightforward iteration and processing mechanism. However, the re-lexing of expression source strings might introduce some overhead compared to handling simpler string literals.

3. **Maintainability**: The separation of text segments and expression source strings into distinct parts simplifies the maintenance of the lexer. Each type of part can be handled separately, reducing the risk of bugs and improving overall code clarity.

In conclusion, the `LexerReaders.cpp` file is a vital component of the Quantum Language compiler, enhancing its functionality with template literals while managing complexity and performance considerations effectively.