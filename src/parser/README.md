# Parser Component of the Quantum Language Compiler

The `parser` component is a crucial part of the Quantum Language compiler, responsible for converting source code into an abstract syntax tree (AST) that can be further processed by the compiler's other components. This component handles the syntactic analysis of the quantum programming language, ensuring that the input code adheres to the language's grammar rules.

## Overview

The parser component consists of several functions and files, each designed to handle specific aspects of the parsing process. These include handling variable declarations, conditional statements, loops, function definitions, and more. The overall flow of the parser involves reading the source code, tokenizing it, and then constructing an AST based on the parsed tokens.

## Files and Functions

### check
- **Description**: Checks if the current token matches a specified type or value.
- **Usage**: Used during token matching to ensure the correct sequence of tokens.

### match
- **Description**: Consumes the next token if it matches a specified type or value.
- **Usage**: Primary function for advancing through the token stream and validating syntax.

### atEnd
- **Description**: Determines if the end of the file has been reached.
- **Usage**: Used to control the loop that processes tokens until the end of the file is reached.

### skipNewlines
- **Description**: Skips over any newline characters in the source code.
- **Usage**: Ensures that the parser correctly handles multi-line statements without interpreting them as separate tokens.

### parse
- **Description**: Main entry point for parsing the entire source code.
- **Usage**: Initializes the parsing process and invokes other parsing functions to construct the AST.

### parseBodyOrStatement
- **Description**: Parses either a block of statements or a single statement.
- **Usage**: Handles the structure of compound statements and individual statements within the program.

### parseVarDecl
- **Description**: Parses variable declarations.
- **Usage**: Constructs AST nodes representing variable declarations.

### parseIfStmt
- **Description**: Parses conditional statements (`if`, `else if`, `else`).
- **Usage**: Builds AST nodes for conditional logic.

### parseWhileStmt
- **Description**: Parses while loops.
- **Usage**: Creates AST nodes for loop structures.

### parseReturnStmt
- **Description**: Parses return statements.
- **Usage**: Generates AST nodes for function exit points.

### parsePrintStmt
- **Description**: Parses print statements.
- **Usage**: Constructs AST nodes for output operations.

### parseInputStmt
- **Description**: Parses input statements.
- **Usage**: Builds AST nodes for receiving user input.

### parseCoutStmt
- **Description**: Parses cout statements (output to console).
- **Usage**: Generates AST nodes for console output.

### parseCinStmt
- **Description**: Parses cin statements (input from console).
- **Usage**: Constructs AST nodes for console input.

### parseImportStmt
- **Description**: Parses import statements.
- **Usage**: Builds AST nodes for module inclusion.

### parseExprStmt
- **Description**: Parses expression statements.
- **Usage**: Constructs AST nodes for executable expressions.

### parseExpr
- **Description**: Parses expressions.
- **Usage**: Recursively constructs AST nodes for complex expressions.

### parseAssignment
- **Description**: Parses assignment operators.
- **Usage**: Builds AST nodes for variable assignments.

### parseOr
- **Description**: Parses logical OR operations.
- **Usage**: Constructs AST nodes for logical OR expressions.

### parseAnd
- **Description**: Parses logical AND operations.
- **Usage**: Builds AST nodes for logical AND expressions.

### parseBitwise
- **Description**: Parses bitwise operations.
- **Usage**: Constructs AST nodes for bitwise operations like AND, OR, XOR.

### parseEquality
- **Description**: Parses equality and inequality comparisons.
- **Usage**: Builds AST nodes for comparison operations like ==, !=.

### parseComparison
- **Description**: Parses relational comparisons.
- **Usage**: Constructs AST nodes for comparison operations like <, >, <=, >=.

### parseShift
- **Description**: Parses shift operations.
- **Usage**: Builds AST nodes for left and right shift operations.

### parseAddSub
- **Description**: Parses addition and subtraction operations.
- **Usage**: Constructs AST nodes for arithmetic operations like +, -.

### parseMulDiv
- **Description**: Parses multiplication and division operations.
- **Usage**: Builds AST nodes for arithmetic operations like *, /.

### parsePower
- **Description**: Parses power operations.
- **Usage**: Constructs AST nodes for exponentiation operations like **.

### parseUnary
- **Description**: Parses unary operations.
- **Usage**: Builds AST nodes for operations like ++, --, !.

### parseArrayLiteral
- **Description**: Parses array literals.
- **Usage**: Constructs AST nodes for array initialization.

### parseDictLiteral
- **Description**: Parses dictionary literals.
- **Usage**: Builds AST nodes for dictionary initialization.

### parseLambda
- **Description**: Parses lambda functions.
- **Usage**: Constructs AST nodes for anonymous functions.

### parseArrowFunction
- **Description**: Parses arrow functions.
- **Usage**: Builds AST nodes for JavaScript-style arrow functions.

### parseArgList
- **Description**: Parses argument lists for functions.
- **Usage**: Constructs AST nodes for function arguments.

### parseParamList
- **Description**: Parses parameter lists for function declarations.
- **Usage**: Builds AST nodes for function parameters.

### isCTypeKeyword
- **Description**: Checks if a given identifier is a keyword in the C-type system.
- **Usage**: Used to validate variable names and prevent conflicts with keywords.

### parseCTypeVarDecl
- **Description**: Parses variable declarations using C-type syntax.
- **Usage**: Constructs AST nodes for C-type variable declarations.

## Directory Structure

```
parser/
├── check.cpp
├── check