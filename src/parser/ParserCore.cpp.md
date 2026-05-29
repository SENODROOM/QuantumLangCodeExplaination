# ParserCore.cpp

## Overview

`ParserCore.cpp` is a central part of the Quantum Language compiler, tasked with transforming the input source code into an Abstract Syntax Tree (AST). It contains the implementation of the `Parser` class, which manages the lexical analysis and syntactic parsing stages of the compilation process.

## Role in Compiler Pipeline

The parser plays a pivotal role in the Quantum Language compiler's pipeline. After the lexer has produced a sequence of tokens, the parser takes these tokens and constructs a structured AST that represents the program's syntax. This AST is then used by subsequent phases of the compiler to perform semantic analysis, optimization, and code generation.

## Key Design Decisions and Why

1. **Token Stream Management**: The parser operates on a stream of tokens provided by the lexer. Managing this token stream efficiently is crucial for accurate parsing. The use of a vector to store tokens allows for easy access and manipulation, such as peeking ahead or consuming tokens as they are processed.

2. **Lexical Analysis Integration**: By integrating lexical analysis directly within the parser, we ensure that the parser receives well-formed tokens. This simplifies error handling and ensures that the parser can focus solely on constructing the AST.

3. **Syntactic Parsing Rules**: The parser uses a set of predefined rules to determine how tokens should be grouped into statements and expressions. These rules are designed based on the grammar of the Quantum Language, ensuring that the resulting AST accurately reflects the intended structure of the code.

4. **Error Handling**: Robust error handling is essential for providing meaningful feedback to the user. The parser throws exceptions when it encounters unexpected tokens or syntax errors, along with detailed error messages indicating the expected token type and what was actually found.

5. **Flexibility and Scalability**: The parser is designed to be flexible and scalable, allowing for easy extension to support new language features. By breaking down the parsing logic into smaller functions, each responsible for parsing a specific construct, the parser remains modular and maintainable.

## Major Classes/Functions Overview

### Parser Class

- **Constructor (`Parser::Parser`)**: Initializes the parser with a vector of tokens and sets the initial position to 0.
- **Current Token Access (`Parser::current`)**: Returns the current token being processed.
- **Peek Ahead (`Parser::peek`)**: Allows looking at a token at a specified offset without advancing the parser.
- **Consume Token (`Parser::consume`)**: Advances the parser to the next token and returns the consumed token.
- **Expect Token (`Parser::expect`)**: Ensures the current token matches the expected type, throwing an exception otherwise.
- **Check Token Type (`Parser::check`)**: Checks if the current token is of a specific type.
- **Match Token Type (`Parser::match`)**: Attempts to match the current token against a specific type, advancing the parser if successful.
- **At End Check (`Parser::atEnd`)**: Determines if the parser has reached the end of the token stream.
- **Skip Newlines (`Parser::skipNewlines`)**: Skips over any consecutive newline tokens, which are often ignored in the AST construction.
- **Parse Function (`Parser::parse`)**: Orchestrates the entire parsing process, starting with a block statement and repeatedly parsing individual statements until the end of the token stream is reached.

### ASTNode Class

The `ASTNode` class serves as the base class for all nodes in the AST. Each node encapsulates a specific type of statement or expression and provides methods for accessing its properties and children.

### Statement and Expression Parsers

The parser includes specialized functions for parsing different types of statements and expressions, such as:
- **Block Statements (`Parser::parseBlock`)**: Parses a sequence of statements enclosed in curly braces.
- **Expression Statements (`Parser::parseExpressionStatement`)**: Parses a single expression followed by a semicolon.
- **Variable Declarations (`Parser::parseVarDeclaration`)**: Parses variable declarations with optional initialization.
- **Function Calls (`Parser::parseFunctionCall`)**: Parses function calls with arguments.
- **Control Flow Statements (`Parser::parseIf`, `Parser::parseWhile`, etc.)**: Parses conditional and looping constructs.

## Tradeoffs

1. **Complexity vs. Simplicity**:
   - **Complexity**: Implementing a comprehensive parser requires understanding and implementing the full grammar of the Quantum Language. This results in more complex code but ensures complete coverage of valid syntax.
   - **Simplicity**: A simpler parser might only handle a subset of the language's features, leading to incomplete or incorrect ASTs. However, this could result in easier-to-maintain code.

2. **Performance vs. Accuracy**:
   - **Performance**: Efficiently managing the token stream and parsing large files quickly is crucial for performance. Optimizing the parser's algorithm can significantly improve processing speed.
   - **Accuracy**: Accurate parsing ensures that the resulting AST correctly represents the program's structure. While some optimizations might sacrifice accuracy, thorough testing and validation help mitigate this risk.

3. **Modularity vs. Monolithic Codebase**:
   - **Modularity**: Breaking down the parsing logic into smaller, reusable functions promotes modularity and makes the code easier to understand and maintain.
   - **Monolithic Codebase**: A monolithic approach consolidates all parsing logic into a single function, potentially reducing complexity but increasing the difficulty of maintenance and scalability.

By carefully balancing these tradeoffs, the Quantum Language compiler's parser aims to provide both robustness and efficiency in converting source code into an AST.