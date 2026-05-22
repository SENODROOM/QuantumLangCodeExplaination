# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is a crucial component of the QuantumLanguage compiler, focusing on the parsing phase of the compilation process. This phase converts the source code into an Abstract Syntax Tree (AST), which serves as the intermediate representation that subsequent phases can operate on.

## Role in Compiler Pipeline

The parser plays a pivotal role in the QuantumLanguage compiler's pipeline. It takes the tokenized input from the lexer and constructs a structured AST. This AST represents the syntactic structure of the source code, making it easier for other parts of the compiler to analyze and transform the code. The parser ensures that the code adheres to the language's grammar rules, handling syntax errors gracefully through custom exception classes like `ParseError`.

## Key Design Decisions and Why

### Pratt Parsing Algorithm

The parser uses the Pratt parsing algorithm for expression evaluation. This algorithm allows for easy extension and modification of operator precedence without complex grammatical changes. By leveraging this approach, the parser can efficiently handle various types of expressions, including arithmetic, bitwise operations, comparisons, and more.

### Lexical Scanning Integration

The parser integrates closely with the lexical scanner (lexer). It relies on the lexer to tokenize the input source code, providing a stream of tokens to work with. This separation of concerns keeps the parsing logic focused on syntax and structure, while the lexer handles the low-level details of character recognition and tokenization.

### Error Handling

Custom exception classes such as `ParseError` are designed to provide detailed information about parsing errors. These exceptions include line and column numbers, allowing developers to pinpoint exactly where the error occurred within the source code. This comprehensive error reporting mechanism enhances debugging and improves the overall user experience.

## Major Classes/Functions Overview

### Parser Class

The `Parser` class is the central class responsible for parsing the source code. It initializes with a vector of tokens and provides a method `parse()` to construct the AST. The class includes several private helper functions to manage token consumption, lookahead, and error handling.

#### Private Helper Functions

- **current()**: Retrieves the current token being processed.
- **peek(int offset)**: Looks ahead at a specified number of tokens without consuming them.
- **consume()**: Consumes the current token and advances the position.
- **expect(TokenType t, const std::string &msg)**: Ensures the next token matches the expected type, throwing an error otherwise.
- **check(TokenType t)**: Checks if the next token matches the specified type without advancing the position.
- **match(TokenType t)**: Matches the next token against the specified type and consumes it if successful.
- **atEnd()**: Determines if all tokens have been consumed.
- **skipNewlines()**: Skips over any newline characters encountered during parsing.

### Parsing Methods

The parser includes a series of methods to handle different types of statements and expressions:

- **parseStatement()**: Parses individual statements.
- **parseBlock()**: Parses blocks of statements enclosed in curly braces `{}`.
- **parseBodyOrStatement()**: Parses either a block or a single statement, optionally allowing brace-optional syntax.
- **parseVarDecl(bool isConst)**: Parses variable declarations, distinguishing between constant and non-constant variables.
- **parseFunctionDecl()**: Parses function declarations, including their parameters and return types.
- **parseClassDecl()**: Parses class declarations, handling inheritance and member variables.
- **parseIfStmt()**, **parseWhileStmt()**, **parseForStmt()**: Parses control flow statements like `if`, `while`, and `for`.
- **parseReturnStmt()**: Parses return statements, extracting the returned value if present.
- **parsePrintStmt()** and **parseInputStmt()**: Parses print and input statements, respectively.
- **parseCoutStmt()** and **parseCinStmt()**: Specialized parsers for formatted output (`cout`) and input (`cin`).
- **parseImportStmt(bool isFrom)**: Handles import statements, supporting both standard imports and imports from specific modules.
- **parseExprStmt()**: Parses expressions as standalone statements.
- **parseCTypeVarDecl(const std::string &typeHint)**: Parses variable declarations with explicit type hints, such as `int x = ...` or `int* p = ...`.
- **isCTypeKeyword(TokenType t)**: Checks if a given token type is a keyword related to C-type declarations.

### Expression Parsing

Expression parsing is implemented using the Pratt parsing algorithm, which supports flexible operator precedence and associativity. The parser includes methods for parsing different levels of expression complexity:

- **parseExpr()**: The main entry point for expression parsing.
- **parseAssignment()**, **parseOr()**, **parseAnd()**: Handle assignment, logical OR, and logical AND operators, respectively.
- **parseBitwise()**, **parseEquality()**, **parseComparison()**: Manage bitwise operations, equality checks, and comparison operators.
- **parseShift()**, **parseAddSub()**, **parseMulDiv()**: Process shift, addition/subtraction, multiplication/division operations.
- **parsePower()**: Handles exponentiation operations.
- **parseUnary()**: Parses unary operators like `!`, `-`, etc.
- **parsePostfix()**: Manages postfix operators such as `x++` or `x--`.
- **parsePrimary()**: Parses basic elements like literals, identifiers, and parentheses.

Additionally, specialized parsers are provided for array literals, dictionary literals, lambdas, and arrow functions:

- **parseArrayLiteral()**
- **parseDictLiteral()**
- **parseLambda()**
- **parseArrowFunction(std::vector<std::string> params, int ln)**

### Parameter List Parsing

The parser also includes functionality to parse parameter lists for functions and classes:

- **parseParamList(std::vector<bool> *outIsRef =