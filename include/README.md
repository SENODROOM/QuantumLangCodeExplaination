# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an essential part of the QuantumLanguage compiler, responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). This parser implements a Pratt parser to handle operator precedence and associativity, ensuring that expressions are correctly parsed according to their rules.

## Role in Compiler Pipeline

The Parser operates as a critical step in the QuantumLanguage compiler's pipeline. It takes the output from the Lexer, which tokenizes the source code, and constructs an AST that represents the structure of the program. The AST is then used by subsequent phases such as semantic analysis, optimization, and code generation.

## Key Design Decisions and Why

1. **Pratt Parser**: The choice of a Pratt parser allows for flexible handling of operator precedence and associativity without requiring complex recursive descent or lookahead techniques. This makes the implementation simpler and more intuitive.

2. **Exception Handling**: The `ParseError` class extends `std::runtime_error`, providing additional information about the error location (`line` and `col`). This helps in diagnosing issues more effectively during development.

3. **Token Helpers**: Functions like `current()`, `peek()`, `consume()`, `expect()`, `check()`, `match()`, `atEnd()`, and `skipNewlines()` provide utility for navigating through the token stream. These functions ensure that the parser can accurately identify and consume tokens as required.

4. **Recursive Descent Parsing**: The parser uses a combination of recursive descent and Pratt parsing to handle different types of statements and expressions. This approach simplifies the parsing logic and makes it easier to extend the language syntax.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: `explicit Parser(std::vector<Token> tokens)` initializes the parser with a vector of tokens.
- **parse() Function**: `ASTNodePtr parse()` parses the entire input and returns the root of the AST.

### Private Member Functions

#### Token Helpers

- **current()**: Returns the current token without advancing the position.
- **peek(int offset = 1)**: Returns the token at the specified offset without advancing the position.
- **consume()**: Consumes the current token and advances the position.
- **expect(TokenType t, const std::string &msg)**: Ensures the next token matches the expected type, throwing a `ParseError` if not.
- **check(TokenType t) const**: Checks if the next token matches the specified type.
- **match(TokenType t)**: If the next token matches the specified type, consumes it and returns true; otherwise, returns false.
- **atEnd() const**: Determines if the end of the token stream has been reached.
- **skipNewlines()**: Skips any newline characters in the token stream.

#### Statement Parsing

- **parseStatement()**: Parses a single statement.
- **parseBlock()**: Parses a block of statements enclosed in curly braces.
- **parseBodyOrStatement()**: Parses either a block or a single statement, allowing for brace-optional blocks.
- **parseVarDecl(bool isConst)**: Parses a variable declaration, optionally specifying whether it should be constant.
- **parseFunctionDecl()**: Parses a function declaration.
- **parseClassDecl()**: Parses a class declaration.
- **parseIfStmt()**, **parseWhileStmt()**, **parseForStmt()**: Parses conditional and loop statements.
- **parseReturnStmt()**: Parses a return statement.
- **parsePrintStmt()** and **parseInputStmt()**: Parses print and input statements.
- **parseCoutStmt()** and **parseCinStmt()**: Parses formatted output and input statements using `cout` and `cin`.
- **parseImportStmt(bool isFrom = false)**: Parses import statements, optionally specifying whether they are from another module.

#### Expression Parsing

- **parseExpr()**: Parses a full expression using Pratt parsing.
- **parseAssignment()**, **parseOr()**, **parseAnd()**, **parseBitwise()**, **parseEquality()**, **parseComparison()**, **parseShift()**, **parseAddSub()**, **parseMulDiv()**, **parsePower()**, **parseUnary()**, **parsePostfix()**, **parsePrimary()**: These functions parse specific types of expressions based on their operators and precedences.

#### Literal Parsing

- **parseArrayLiteral()** and **parseDictLiteral()**: Parses array and dictionary literals respectively.
- **parseLambda()**: Parses lambda functions.
- **parseArrowFunction(std::vector<std::string> params, int ln)**: Parses arrow functions with specified parameters and line number.
- **parseArgList()**: Parses a list of arguments for function calls or declarations.
- **parseParamList(std::vector<bool> *outIsRef = nullptr, std::vector<ASTNodePtr> *outDefaultArgs = nullptr, std::vector<std::string> *outParamTypes = nullptr)**: Parses a list of parameters for function declarations, populating optional vectors with reference status, default arguments, and parameter types.

## Tradeoffs

1. **Complexity vs. Flexibility**: While the Pratt parser provides flexibility in handling operator precedence, it might add some complexity compared to other parsing approaches. However, this tradeoff is deemed acceptable for its simplicity and ease of extension.

2. **Performance**: Recursive descent parsers can be less efficient than Pratt parsers due to potential stack overflows and increased overhead. However, the performance impact is mitigated by careful implementation and use of tail recursion where possible.

3. **Readability**: The use of token helpers and clear separation of concerns between different parsing stages improves readability and maintainability of the codebase.

By leveraging these design decisions and implementing robust parsing mechanisms, the `Parser.h` header file ensures that the QuantumLanguage compiler can efficiently and accurately convert source code