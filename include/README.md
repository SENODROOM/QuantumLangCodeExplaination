# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an essential part of the QuantumLanguage compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). The parser handles lexical analysis, grammar rules, and semantic checks to ensure that the source code adheres to the language's syntax and semantics. This process is critical for compiling QuantumLanguage programs efficiently and accurately.

## Role in Compiler Pipeline

The parser operates as the second stage in the compiler pipeline after lexical analysis. It takes a sequence of tokens produced by the lexer and constructs a structured representation of the source code in the form of an AST. The AST serves as the foundation for subsequent stages such as semantic analysis, optimization, and code generation.

## Key Design Decisions and Why

1. **Exception Handling**: The parser uses custom exception classes (`ParseError`) to handle errors during parsing. These exceptions store the line and column numbers where the error occurred, providing more precise error reporting.

2. **Precedence Parsing (Pratt Parsing)**: To manage operator precedence correctly, the parser employs Pratt parsing. This technique allows operators to be parsed based on their associativity and precedence levels, making it easier to handle complex expressions without ambiguity.

3. **Recursive Descent Parsing**: The parser uses a recursive descent approach to implement the grammar rules. This method simplifies the implementation of the parser by breaking down the grammar into smaller, manageable functions, each corresponding to a production rule.

4. **Flexibility in Statement Parsing**: The parser distinguishes between blocks and single statements, allowing for flexible parsing depending on the context. This feature supports both brace-enclosed blocks and individual statements, enhancing the language's usability.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: `explicit Parser(std::vector<Token> tokens)` - Initializes the parser with a vector of tokens.
- **parse Method**: `ASTNodePtr parse()` - Parses the entire source code and returns the root node of the AST.

### Private Methods

#### Token Helpers

- **current Method**: `Token &current()` - Returns the current token being processed.
- **peek Method**: `Token &peek(int offset = 1)` - Returns the token at the specified offset without advancing the parser position.
- **consume Method**: `Token &consume()` - Advances the parser position and returns the consumed token.
- **expect Method**: `Token &expect(TokenType t, const std::string &msg)` - Ensures the next token matches the expected type, throwing a `ParseError` if not.
- **check Method**: `bool check(TokenType t) const` - Checks if the next token matches the given type.
- **match Method**: `bool match(TokenType t)` - Consumes the next token if it matches the given type.
- **atEnd Method**: `bool atEnd() const` - Determines if the parser has reached the end of the token stream.
- **skipNewlines Method**: `void skipNewlines()` - Skips any newline characters encountered in the token stream.

#### Statement Parsing

- **parseStatement Method**: `ASTNodePtr parseStatement()` - Parses a single statement.
- **parseBlock Method**: `ASTNodePtr parseBlock()` - Parses a block of statements enclosed in curly braces.
- **parseBodyOrStatement Method**: `ASTNodePtr parseBodyOrStatement()` - Parses either a block or a single statement, supporting brace-optional syntax.

#### Declaration Parsing

- **parseVarDecl Method**: `ASTNodePtr parseVarDecl(bool isConst)` - Parses variable declarations, including constant declarations.
- **parseFunctionDecl Method**: `ASTNodePtr parseFunctionDecl()` - Parses function declarations.
- **parseClassDecl Method**: `ASTNodePtr parseClassDecl()` - Parses class declarations.

#### Control Flow Statements

- **parseIfStmt Method**: `ASTNodePtr parseIfStmt()` - Parses `if` statements.
- **parseWhileStmt Method**: `ASTNodePtr parseWhileStmt()` - Parses `while` loops.
- **parseForStmt Method**: `ASTNodePtr parseForStmt()` - Parses `for` loops.
- **parseReturnStmt Method**: `ASTNodePtr parseReturnStmt()` - Parses return statements.
- **parsePrintStmt Method**: `ASTNodePtr parsePrintStmt()` - Parses print statements.
- **parseInputStmt Method**: `ASTNodePtr parseInputStmt()` - Parses input statements.

#### I/O Operations

- **parseCoutStmt Method**: `ASTNodePtr parseCoutStmt()` - Parses `cout` operations.
- **parseCinStmt Method**: `ASTNodePtr parseCinStmt()` - Parses `cin` operations.

#### Import Statements

- **parseImportStmt Method**: `ASTNodePtr parseImportStmt(bool isFrom = false)` - Parses import statements, optionally supporting `from` syntax.

#### Expression Parsing

- **parseExpr Method**: `ASTNodePtr parseExpr()` - Parses expressions using Pratt parsing.
- **parseAssignment Method**: `ASTNodePtr parseAssignment()` - Parses assignment expressions.
- **parseOr Method**: `ASTNodePtr parseOr()` - Parses logical OR expressions.
- **parseAnd Method**: `ASTNodePtr parseAnd()` - Parses logical AND expressions.
- **parseBitwise Method**: `ASTNodePtr parseBitwise()` - Parses bitwise expressions.
- **parseEquality Method**: `ASTNodePtr parseEquality()` - Parses equality expressions.
- **parseComparison Method**: `ASTNodePtr parseComparison()` - Parses comparison expressions.
- **parseShift Method**: `ASTNodePtr parseShift()` - Parses shift expressions.
- **parseAddSub Method**: `ASTNodePtr parseAddSub()` - Parses addition and subtraction expressions.
- **parseMulDiv Method**: `ASTNodePtr parseMulDiv()` - Parses multiplication and division expressions.
- **parsePower Method**: `