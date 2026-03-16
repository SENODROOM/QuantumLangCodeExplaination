# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is a critical component of the QuantumLanguage compiler, focusing on the parsing phase of the compilation process. This file defines the `Parser` class, which takes a vector of `Token`s as input and constructs an Abstract Syntax Tree (AST) representing the syntactic structure of the source code. The parser employs a Pratt parser algorithm to manage operator precedence and associativity, ensuring that complex expressions are correctly interpreted.

## Role in Compiler Pipeline

The `Parser` class operates during the lexical analysis phase of the compiler, following the tokenization step. It converts the sequence of tokens into a structured representation (AST) that can be further analyzed and transformed into machine code or bytecode. The parser is essential for accurately interpreting the source code's syntax, facilitating subsequent stages such as semantic analysis and code generation.

## Key Design Decisions and Why

### Pratt Parser Algorithm

The parser uses the Pratt parser algorithm, which allows for flexible handling of operator precedence and associativity. This approach avoids the need for recursive descent parsers to explicitly manage operator precedence rules, simplifying the implementation and reducing potential errors.

### Error Handling

A custom `ParseError` exception class is defined to handle parsing errors. This exception includes line and column information, making it easier to pinpoint the exact location of errors within the source code. The use of exceptions for error handling ensures that the parser can gracefully report errors and terminate without proceeding with invalid syntax.

### Flexibility in Parsing Declarations

The parser supports various declarations including variable, function, and class declarations. Each declaration type has its own parsing method (`parseVarDecl`, `parseFunctionDecl`, etc.), allowing for clear separation of concerns and easy maintenance. Additionally, the parser handles both mutable and constant variable declarations, enhancing flexibility and readability.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: Initializes the parser with a vector of tokens.
- **parse() Method**: Main entry point for parsing the entire source code, returning an ASTNodePtr representing the root of the parsed AST.

### Token Helpers

- **current()**: Retrieves the current token being processed.
- **peek(int offset)**: Looks ahead at a token relative to the current position.
- **consume()**: Advances to the next token and returns it.
- **expect(TokenType t, const std::string &msg)**: Consumes the expected token, throwing a `ParseError` if the token does not match.
- **check(TokenType t)**: Checks if the next token matches the specified type.
- **match(TokenType t)**: Consumes the next token if it matches the specified type.
- **atEnd()**: Determines if the end of the token stream has been reached.
- **skipNewlines()**: Skips over any newline characters encountered during parsing.

### Parsing Methods

- **parseStatement()**: Parses a single statement from the token stream.
- **parseBlock()**: Parses a block of statements enclosed in curly braces.
- **parseBodyOrStatement()**: Parses either a block or a single statement, depending on whether curly braces are present.
- **parseVarDecl(bool isConst)**: Parses a variable declaration, optionally marking it as constant.
- **parseFunctionDecl()**: Parses a function declaration, including parameter lists and return types.
- **parseClassDecl()**: Parses a class declaration, including member variables and methods.
- **parseIfStmt()**, **parseWhileStmt()**, **parseForStmt()**: Parses conditional and loop statements.
- **parseReturnStmt()**: Parses a return statement, extracting the returned expression.
- **parsePrintStmt()** and **parseInputStmt()**: Parses print and input statements respectively.
- **parseCoutStmt()** and **parseCinStmt()**: Parses formatted output and input statements using `cout` and `cin`.
- **parseImportStmt(bool isFrom)**: Parses import statements, supporting both standard imports and imports from specific modules.
- **parseExprStmt()**: Parses an expression followed by a semicolon.
- **parseCTypeVarDecl(const std::string &typeHint)**: Parses variable declarations with explicit type hints, supporting pointer types.
- **isCTypeKeyword(TokenType t)**: Checks if a token represents a C-like type keyword.

### Expression Parsing

The parser includes multiple methods for parsing different types of expressions, adhering to the Pratt parser style:

- **parseExpr()**
- **parseAssignment()**
- **parseOr(), parseAnd()**
- **parseBitwise()**
- **parseEquality(), parseComparison()**
- **parseShift()**
- **parseAddSub()**
- **parseMulDiv()**
- **parsePower()**
- **parseUnary()**
- **parsePostfix()**
- **parsePrimary()**

Additionally, specialized methods for parsing array literals, dictionary literals, lambdas, and arrow functions are provided.

## Tradeoffs

### Complexity vs. Simplicity

Using the Pratt parser algorithm simplifies the implementation of operator precedence handling but may increase complexity compared to recursive descent parsers. However, this trade-off is justified by the reduced likelihood of errors and improved maintainability.

### Error Reporting

The use of exceptions for error reporting provides a powerful mechanism for pinpointing errors but can also lead to increased overhead due to exception handling. Balancing comprehensive error reporting with performance considerations is a key challenge addressed by the design.

### Flexibility vs. Consistency

Supporting various declaration types and providing flexibility in parsing expressions enhances the language's usability but may introduce inconsistencies in how similar structures are handled. Ensuring consistency while maintaining flexibility requires careful consideration of the parser's design.

In summary, the `Parser.h` header file plays a pivotal role in the QuantumLanguage compiler by converting tokenized source code into a structured AST. Its use of the Pratt parser algorithm, combined with robust error handling and support for various declar