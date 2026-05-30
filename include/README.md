# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is a crucial component of the QuantumLanguage compiler, responsible for converting the tokenized input into an Abstract Syntax Tree (AST). The parser follows a top-down approach with recursive descent to construct the AST based on the grammar rules defined for the language. This file includes various classes and functions designed to handle different aspects of parsing, such as handling statements, expressions, and declarations.

## Role in Compiler Pipeline

The parser operates during the compilation process, following the lexical analysis phase where tokens are generated from the source code. Its primary role is to take these tokens and construct a structured representation of the program's syntax, which is then used by other phases like semantic analysis and code generation.

### Key Design Decisions and Why

1. **Top-Down Recursive Descent**: This approach simplifies the implementation of the parser by breaking down the problem into smaller subproblems. Each function corresponds to a non-terminal in the grammar, making it easier to understand and maintain.

2. **Precedence Climb Algorithm**: Used for parsing expressions with operators of varying precedence levels. This algorithm allows the parser to handle complex expressions without requiring extensive lookahead or backtracking.

3. **Error Handling**: The parser throws exceptions (`ParseError`) when it encounters errors, providing detailed information about the error location and message. This helps in debugging and improving the robustness of the compiler.

## Major Classes/Functions Overview

### Parser Class

- **Constructor (`explicit Parser(std::vector<Token> tokens)`)**: Initializes the parser with a vector of tokens.
- **parse Method (`ASTNodePtr parse()`)**: Parses the entire input and returns the root node of the AST.

### Private Methods

#### Token Helpers

- **current Method**: Returns the current token being processed.
- **peek Method**: Returns the token at a specified offset ahead in the token stream.
- **consume Method**: Consumes the current token and advances the position.
- **expect Method**: Ensures the next token matches the expected type, throwing an exception otherwise.
- **check Method**: Checks if the next token matches a specific type without advancing the position.
- **match Method**: Matches the next token against a specific type and consumes it if matched.
- **atEnd Method**: Determines if the end of the token stream has been reached.
- **skipNewlines Method**: Skips any newline tokens encountered.

#### Statement Parsing

- **parseStatement Method**: Parses a single statement.
- **parseBlock Method**: Parses a block of statements enclosed in curly braces.
- **parseBodyOrStatement Method**: Parses either a block or a single statement, allowing for brace-optional blocks.
- **parseVarDecl Method**: Parses variable declarations, optionally marking them as constant.
- **parseFunctionDecl Method**: Parses function declarations.
- **parseClassDecl Method**: Parses class declarations.
- **parseIfStmt Method**: Parses conditional statements.
- **parseWhileStmt Method**: Parses while loops.
- **parseForStmt Method**: Parses for loops.
- **parseReturnStmt Method**: Parses return statements.
- **parsePrintStmt Method**: Parses print statements.
- **parseInputStmt Method**: Parses input statements.
- **parseCoutStmt Method**: Parses output redirection using `cout`.
- **parseCinStmt Method**: Parses input redirection using `cin`.
- **parseImportStmt Method**: Parses import statements, optionally specifying a module name.
- **parseExprStmt Method**: Parses expression statements.

#### Expression Parsing

- **parseExpr Method**: Parses expressions using the Pratt-style precedence climbing algorithm.
- **parseAssignment Method**: Parses assignment expressions.
- **parseOr Method**: Parses logical OR expressions.
- **parseAnd Method**: Parses logical AND expressions.
- **parseBitwise Method**: Parses bitwise operations.
- **parseEquality Method**: Parses equality and inequality comparisons.
- **parseComparison Method**: Parses relational comparisons.
- **parseShift Method**: Parses shift operations.
- **parseAddSub Method**: Parses addition and subtraction.
- **parseMulDiv Method**: Parses multiplication and division.
- **parsePower Method**: Parses exponentiation.
- **parseUnary Method**: Parses unary operators.
- **parsePostfix Method**: Parses postfix expressions.
- **parsePrimary Method**: Parses primary expressions (variables, literals, etc.).

#### Literal Parsing

- **parseArrayLiteral Method**: Parses array literal expressions.
- **parseDictLiteral Method**: Parses dictionary literal expressions.
- **parseLambda Method**: Parses lambda function expressions.
- **parseArrowFunction Method**: Parses arrow function expressions.
- **parseArgList Method**: Parses argument lists for functions and lambdas.
- **parseParamList Method**: Parses parameter lists for functions, including handling reference parameters and default arguments.

## Tradeoffs

- **Complexity vs. Simplicity**: While the top-down recursive descent approach makes the parser relatively simple to implement, it can become cumbersome as the grammar becomes more complex.
  
- **Performance**: The Pratt-style precedence climbing algorithm offers good performance for parsing expressions but may require additional memory due to its stack-based nature.

- **Readability**: The separation of concerns between different types of parsing (statements, expressions, etc.) enhances readability but adds complexity to the overall structure of the parser.

Overall, the `Parser.h` file plays a vital role in the QuantumLanguage compiler by transforming the raw tokenized input into a structured AST, enabling subsequent phases to perform semantic analysis and code generation efficiently.