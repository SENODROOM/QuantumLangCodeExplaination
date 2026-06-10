# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an integral part of the QuantumLanguage compiler, focusing on the parsing phase of the compilation process. This phase converts the source code into an Abstract Syntax Tree (AST) which represents the structure of the program. The `Parser` class handles the lexical analysis and syntactic analysis to construct the AST.

## Role in Compiler Pipeline

The parser operates as a critical component within the QuantumLanguage compiler's pipeline. It follows the lexer, which breaks down the source code into individual tokens. The parser then takes these tokens and constructs a structured representation of the program in the form of an AST. This AST serves as the foundation for subsequent phases such as semantic analysis, optimization, and code generation.

## Key Design Decisions and Why

### Pratt Parsing Algorithm

The parser uses the Pratt parsing algorithm for expression parsing. This algorithm allows for easy implementation of operator precedence and associativity rules. By using this approach, the compiler can handle complex expressions without requiring extensive lookahead or recursion, making it efficient and easier to implement.

### Error Handling

A custom exception class `ParseError` is defined to handle parsing errors. This class inherits from `std::runtime_error` and includes additional information about the error location (`line` and `col`). This helps in providing more precise error messages during the compilation process, aiding developers in debugging their code effectively.

## Major Classes/Functions Overview

### Parser Class

The `Parser` class is central to the parsing phase. It is initialized with a vector of tokens produced by the lexer. The main function `parse()` drives the entire parsing process, converting the sequence of tokens into an AST.

#### Private Member Functions

- **Token Helpers**: These functions assist in navigating through the token stream, including getting the current token, peeking ahead, consuming tokens, expecting specific token types, checking token presence, matching tokens, skipping newlines, etc.
  
- **Parsing Methods**: These methods correspond to different types of statements and declarations found in the source code. They include parsing variable declarations, function declarations, class declarations, control flow statements like `if`, `while`, and `for`, return statements, print statements, input statements, and import statements.

- **Expression Parsing**: The parser also handles the parsing of expressions using the Pratt parsing algorithm. It includes methods for parsing various operators and literals, ensuring correct precedence and associativity.

### Trade-offs

- **Efficiency vs. Complexity**: Using the Pratt parsing algorithm simplifies the implementation but may increase complexity slightly compared to other approaches like recursive descent parsers. However, the benefits in terms of readability and ease of extension outweigh the potential drawbacks.

- **Error Reporting**: The custom `ParseError` class provides detailed error reporting, which enhances the developer experience but adds overhead to the parser implementation.

In conclusion, the `Parser.h` file is a vital component of the QuantumLanguage compiler, responsible for transforming the source code into a structured AST. Its design choices, particularly the use of the Pratt parsing algorithm, ensure both efficiency and robust error handling, making it a cornerstone of the compiler's functionality.