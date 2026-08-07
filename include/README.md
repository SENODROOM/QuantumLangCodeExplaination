# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an integral part of the QuantumLanguage compiler, focusing on the parsing phase of the compilation process. This phase converts the source code into an Abstract Syntax Tree (AST), which serves as the intermediate representation used during subsequent phases such as semantic analysis and code generation.

## Role in Compiler Pipeline

The parser's role in the compiler pipeline is critical as it takes the input source code and breaks it down into meaningful units called tokens. These tokens are then organized into a structured format represented by the AST. The parser handles syntax errors, manages token streams, and invokes appropriate parsing functions based on the grammar rules defined for the language.

## Key Design Decisions and Why

1. **Exception Handling**: The parser throws custom exceptions (`ParseError`) when syntax errors occur, providing detailed error messages including line and column numbers. This helps developers quickly identify and fix issues in their code.

2. **Pratt-Parsing Algorithm**: For expression parsing, the parser uses the Pratt-parsing algorithm, which allows for flexible operator precedence parsing. This approach simplifies the implementation of complex expressions without requiring extensive manual handling of operator associativity and precedence.

3. **Recursive Descent Parsing**: The parser employs a recursive descent strategy, where each non-terminal symbol in the grammar is parsed by a separate function. This method is straightforward and easy to understand, making it suitable for languages with relatively simple grammars.

4. **Brace-Optional Blocks**: The parser supports both brace-enclosed blocks and single statements without braces, enhancing the flexibility of the language and reducing boilerplate code.

5. **Desugaring of Control Structures**: Complex control structures like `switch` are desugared into simpler constructs (`if` chains) during parsing. This reduces the complexity of later stages of the compiler and makes the code easier to handle.

## Major Classes/Functions Overview

### Class: Parser

- **Constructor**: Initializes the parser with a vector of tokens.
- **parse()**: Main entry point for parsing the entire source code into an AST.

### Private Member Functions:

- **Token Helpers**:
  - `current()`: Returns the current token being processed.
  - `peek(int offset = 1)`: Returns the token at a specified offset ahead in the stream.
  - `consume()`: Consumes the current token and advances to the next one.
  - `expect(TokenType t, const std::string &msg)`: Ensures the current token matches the expected type, throwing an exception otherwise.
  - `check(TokenType t) const`: Checks if the current token matches the specified type without advancing.
  - `match(TokenType t)`: Matches the current token against the specified type and advances if they match.
  - `atEnd() const`: Determines if the end of the token stream has been reached.
  - `skipNewlines()`: Skips any newline characters encountered in the token stream.

- **Parsing Methods**:
  - `parseStatement()`, `parseBlock()`, `parseBodyOrStatement()`: Handle different types of statements and blocks.
  - `parseVarDecl(bool isConst)`, `parseFunctionDecl()`, `parseClassDecl()`: Parse variable declarations, function definitions, and class definitions.
  - `parseIfStmt()`, `parseWhileStmt()`, `parseForStmt()`, `parseSwitchStmt()`, `parseReturnStmt()`: Implement control flow structures.
  - `parsePrintStmt()`, `parseInputStmt()`, `parseCoutStmt()`, `parseCinStmt()`: Handle output and input operations.
  - `parseImportStmt(bool isFrom = false)`: Manage import statements.
  - `parseExprStmt()`: Parse standalone expressions.
  - `parseCTypeVarDecl(const std::string &typeHint)`: Support variable declarations with type hints.
  - `isCTypeKeyword(TokenType t) const`: Check if a token represents a C-type keyword.

- **Expression Parsing**:
  - `parseExpr()`, `parseAssignment()`, `parseOr()`, `parseAnd()`, `parseBitwise()`, `parseEquality()`, `parseComparison()`, `parseShift()`, `parseAddSub()`, `parseMulDiv()`, `parsePower()`, `parseUnary()`, `parsePostfix()`, `parsePrimary()`: Implement various levels of operator precedence parsing.
  - `parseArrayLiteral()`, `parseDictLiteral()`, `parseLambda()`, `parseArrowFunction(std::vector<std::string> params, int ln)`: Handle specific literal and function types.

- **Utility Functions**:
  - `parseArgList()`, `parseParamList(std::vector<bool> *outIsRef = nullptr, std::vector<ASTNodePtr> *outDefaultArgs = nullptr, std::vector<std::string> *outParamTypes = nullptr)`: Helper functions to parse argument lists and parameter lists.

## Tradeoffs

- **Flexibility vs. Complexity**: Using the Pratt-parsing algorithm provides flexibility but can increase the complexity of the parser implementation.
- **Error Handling**: Detailed error messages improve developer experience but may add overhead during parsing.
- **Desugaring**: Simplifying complex control structures reduces complexity but may obscure the original intent of the code.
- **Operator Precedence**: The Pratt-parsing algorithm offers a powerful way to handle operator precedence but requires careful implementation to avoid common pitfalls.

Overall, the `Parser.h` file is designed to be robust, flexible, and maintainable, ensuring efficient parsing of QuantumLanguage source code into an AST.