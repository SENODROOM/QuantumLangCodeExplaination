# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an integral component of the QuantumLanguage compiler, responsible for converting sequences of tokens into an Abstract Syntax Tree (AST). This file plays a crucial role in the compiler's pipeline by interpreting the syntactic structure of the source code and constructing a hierarchical representation of the program's logic.

## Key Design Decisions

### Use of `std::variant` for Value Types

The decision to use `std::variant` was driven by the need to manage multiple data types within the language efficiently. By encapsulating different value types within a single variant, the parser can easily switch between these types without the overhead of dynamic casting or inheritance hierarchies. This approach enhances performance and simplifies the implementation of type handling operations.

### Pratt Parsing Algorithm for Expressions

Pratt parsing was chosen as the method for parsing expressions due to its ability to handle operator precedence and associativity cleanly. The algorithm allows for recursive descent parsing while maintaining control over the precedence of operators, which is essential for correctly evaluating complex mathematical expressions and logical conditions.

## Documentation

### Class: `ParseError`

**Purpose:** Represents an error encountered during parsing, providing both a message and the location (line and column) where the error occurred.

**Behavior:** Inherits from `std::runtime_error` and adds additional attributes for line and column numbers.

### Class: `Parser`

**Purpose:** Manages the parsing process, converting a sequence of tokens into an AST.

**Behavior:** 
- Initializes with a vector of tokens.
- Provides a method `parse()` to generate the AST.
- Utilizes helper functions to consume tokens, check token types, and perform specific parsing tasks.
- Implements Pratt parsing for expression evaluation, ensuring correct precedence and associativity.

#### Private Member Functions:

- **`Token &current()`**: Returns the current token being processed.
- **`Token &peek(int offset = 1)`**: Returns the token at a specified offset ahead in the token stream.
- **`Token &consume()`**: Consumes the current token and advances the position.
- **`Token &expect(TokenType t, const std::string &msg)`**: Ensures the next token matches the expected type, throwing an error if not.
- **`bool check(TokenType t) const`**: Checks if the next token matches the specified type.
- **`bool match(TokenType t)`**: Consumes the next token if it matches the specified type.
- **`bool atEnd() const`**: Determines if the end of the token stream has been reached.
- **`void skipNewlines()`**: Skips any newline characters in the token stream.

#### Parsing Methods:

- **`ASTNodePtr parseStatement()`**: Parses a single statement.
- **`ASTNodePtr parseBlock()`**: Parses a block of statements enclosed in curly braces.
- **`ASTNodePtr parseBodyOrStatement()`**: Parses either a block or a single statement, allowing for optional braces.
- **`ASTNodePtr parseVarDecl(bool isConst)`**: Parses variable declarations, optionally marking them as constant.
- **`ASTNodePtr parseFunctionDecl()`**: Parses function declarations.
- **`ASTNodePtr parseClassDecl()`**: Parses class declarations.
- **`ASTNodePtr parseIfStmt()`**: Parses conditional statements (`if`).
- **`ASTNodePtr parseWhileStmt()`**: Parses loop statements (`while`).
- **`ASTNodePtr parseForStmt()`**: Parses loop statements (`for`).
- **`ASTNodePtr parseReturnStmt()`**: Parses return statements.
- **`ASTNodePtr parsePrintStmt()`**: Parses print statements.
- **`ASTNodePtr parseInputStmt()`**: Parses input statements.
- **`ASTNodePtr parseCoutStmt()`**: Parses output redirection statements using `cout`.
- **`ASTNodePtr parseCinStmt()`**: Parses input redirection statements using `cin`.
- **`ASTNodePtr parseImportStmt(bool isFrom = false)`**: Parses import statements, optionally specifying a module.
- **`ASTNodePtr parseExprStmt()`**: Parses expressions as standalone statements.
- **`ASTNodePtr parseCTypeVarDecl(const std::string &typeHint)`**: Parses variable declarations with a specified type hint.
- **`bool isCTypeKeyword(TokenType t) const`**: Checks if a given token type represents a C-type keyword.

#### Expression Parsing:

- **`ASTNodePtr parseExpr()`**: Entry point for parsing expressions using Pratt parsing.
- **`ASTNodePtr parseAssignment()`**: Parses assignment expressions.
- **`ASTNodePtr parseOr()`**: Parses logical OR expressions.
- **`ASTNodePtr parseAnd()`**: Parses logical AND expressions.
- **`ASTNodePtr parseBitwise()`**: Parses bitwise operation expressions.
- **`ASTNodePtr parseEquality()`**: Parses equality comparison expressions.
- **`ASTNodePtr parseComparison()`**: Parses relational comparison expressions.
- **`ASTNodePtr parseShift()`**: Parses shift operation expressions.
- **`ASTNodePtr parseAddSub()`**: Parses addition and subtraction expressions.
- **`ASTNodePtr parseMulDiv()`**: Parses multiplication and division expressions.
- **`ASTNodePtr parsePower()`**: Parses exponentiation expressions.
- **`ASTNodePtr parseUnary()`**: Parses unary operation expressions.
- **`ASTNodePtr parsePostfix()`**: Parses postfix operation expressions.
- **`ASTNodePtr parsePrimary()`**: Parses primary expressions (literals, variables, etc.).

#### Literal Parsing:

- **`ASTNodePtr parseArrayLiteral()`**: Parses array literals.
- **`ASTNodePtr parseDictLiteral()`**: Parses dictionary literals.
- **`ASTNodePtr parseLambda()`**: Parses lambda expressions.
- **`ASTNodePtr parseArrowFunction(std::vector<std::string> params, int ln)`**: Parses arrow