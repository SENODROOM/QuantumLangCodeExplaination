# ParserCore.cpp

## Overview

`ParserCore.cpp` is a critical component of the Quantum Language compiler, responsible for transforming the input source code into an Abstract Syntax Tree (AST). This file contains the implementation of the `Parser` class, which manages both the lexical analysis and syntactic parsing stages of the compilation process.

## Role in Compiler Pipeline

The parser plays a pivotal role in the Quantum Language compiler by converting the raw text of the source code into a structured representation known as an AST. This AST serves as the foundation for subsequent stages of the compilation process, including semantic analysis and code generation.

### Key Design Decisions and Why

1. **Tokenization**: The parser relies on pre-tokenized input to simplify its task. This decision avoids the complexity of implementing a lexer within the parser itself, allowing for more focused development on the syntactic rules.

2. **Recursive Descent Parsing**: The parser uses a recursive descent approach, where each function corresponds to a grammar rule. This method ensures that the parser can handle complex grammars with ease and provides clear insights into how the language is parsed.

3. **Lookahead Mechanism**: To manage non-LL(1) grammars, the parser implements a lookahead mechanism using the `peek()` function. This allows the parser to look ahead at upcoming tokens without consuming them, enabling it to make informed decisions based on future syntax.

4. **Error Handling**: The parser includes robust error handling mechanisms through the `expect()` and `match()` functions. These functions help in identifying and reporting syntax errors gracefully, guiding developers towards correcting their code.

## Major Classes/Functions Overview

### Parser Class

- **Constructor (`Parser::Parser(std::vector<Token> tokens)`)**:
  - Initializes the parser with a vector of tokens obtained from lexical analysis.

- **Current Token Access (`Parser::current()`)**:
  - Returns the current token being processed.

- **Peek Ahead (`Parser::peek(int offset)`)**:
  - Allows accessing a token at a specified offset ahead in the sequence without advancing the position.

- **Consume Current Token (`Parser::consume()`)**:
  - Advances the parser's position to the next token and returns the current one.

- **Expect Specific Token Type (`Parser::expect(TokenType t, const std::string &msg)`)**:
  - Checks if the current token matches the expected type. If not, throws a `ParseError` with a custom message indicating what was expected and what was encountered.

- **Check Token Type (`Parser::check(TokenType t)`)**:
  - Returns `true` if the current token matches the specified type.

- **Match Token Type (`Parser::match(TokenType t)`)**:
  - Consumes the current token if it matches the specified type, otherwise returns `false`.

- **Skip Newlines (`Parser::skipNewlines()`)**:
  - Continuously consumes newline tokens until the end of the input or a non-newline token is encountered.

- **Main Parsing Function (`Parser::parse()`)**:
  - Constructs the AST by repeatedly calling `parseStatement()` until the end of the input is reached. It also skips any newlines between statements.

### ASTNode Class

This class represents nodes in the Abstract Syntax Tree (AST), serving as the building blocks for the structured representation of the source code. Each node encapsulates information about a specific part of the syntax, such as expressions, statements, or declarations.

## Tradeoffs

### Advantages

- **Simplicity**: By separating lexical analysis and parsing, the codebase becomes cleaner and easier to maintain.
- **Flexibility**: Recursive descent parsing allows for straightforward implementation of complex grammars.
- **Readability**: Clear separation of concerns makes the code easier to understand and debug.

### Disadvantages

- **Performance Overhead**: The lookahead mechanism adds some overhead compared to simpler parsers like LL(1).
- **Increased Complexity**: Managing multiple lookahead levels can lead to more intricate code, potentially increasing maintenance costs.

Overall, the design of `ParserCore.cpp` strikes a balance between simplicity and functionality, making it well-suited for the Quantum Language compiler's needs.