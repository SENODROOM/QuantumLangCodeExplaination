# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an essential part of the QuantumLanguage compiler, focusing on the parsing phase. The parser converts source code into an Abstract Syntax Tree (AST), which represents the syntactic structure of the program. This AST is then used by other components of the compiler to generate executable code or perform semantic analysis.

## Role in Compiler Pipeline

The parser operates during the lexical analysis phase, where it takes a sequence of tokens produced by the lexer and constructs an AST. It plays a crucial role in ensuring that the source code adheres to the language's grammar rules, thereby facilitating subsequent stages such as semantic analysis and code generation.

### Key Design Decisions and Why

1. **Exception Handling**: The parser throws custom `ParseError` exceptions when syntax errors are encountered. This allows for precise error reporting, including the line and column numbers where the error occurred, enhancing developer experience.
   
2. **Pratt Parsing Algorithm**: For expression parsing, the parser employs the Pratt parsing algorithm, which is known for its efficiency and ability to handle operator precedence without recursion. This decision simplifies the implementation and improves performance.

3. **Recursive Descent Parser**: The parser uses a recursive descent approach, breaking down the grammar into smaller, manageable parts. Each part corresponds to a function within the parser class, making the code easier to understand and maintain.

4. **Flexibility in Statement Types**: The parser supports various types of statements, including variable declarations, function definitions, control structures (if, while, for), return statements, print statements, input statements, and more. This flexibility ensures that the compiler can handle a wide range of quantum programming constructs.

## Major Classes/Functions Overview

### Parser Class

- **Constructor (`explicit Parser(std::vector<Token> tokens)`)**: Initializes the parser with a vector of tokens.
  
- **Public Method (`ASTNodePtr parse()`)**: Parses the entire input stream and returns the root of the AST.

- **Private Methods**:
  - **Token Helpers**: Functions like `current()`, `peek()`, `consume()`, `expect()`, `check()`, `match()`, `atEnd()`, and `skipNewlines()` assist in navigating through the token stream.
  - **Parsing Statements**: Functions such as `parseStatement()`, `parseBlock()`, `parseBodyOrStatement()`, `parseVarDecl()`, `parseFunctionDecl()`, `parseClassDecl()`, `parseIfStmt()`, `parseWhileStmt()`, `parseForStmt()`, `parseReturnStmt()`, `parsePrintStmt()`, `parseInputStmt()`, `parseCoutStmt()`, `parseCinStmt()`, `parseImportStmt()`, and `parseExprStmt()` handle different kinds of statements.
  - **Expression Parsing**: Functions like `parseExpr()`, `parseAssignment()`, `parseOr()`, `parseAnd()`, `parseBitwise()`, `parseEquality()`, `parseComparison()`, `parseShift()`, `parseAddSub()`, `parseMulDiv()`, `parsePower()`, `parseUnary()`, `parsePostfix()`, and `parsePrimary()` implement the Pratt parsing algorithm to handle expressions.

### ParseError Class

- **Constructor (`ParseError(const std::string &msg, int l, int c)`)**: Creates a new `ParseError` object with a message, line number, and column number.

## Tradeoffs

1. **Complexity vs. Performance**: While the recursive descent parser is straightforward and easy to understand, it may not be as efficient as parsers using more advanced techniques like LL(k) or LR(k). However, the simplicity makes it easier to debug and extend.

2. **Error Reporting**: Custom exception handling provides detailed error messages but adds overhead compared to simpler error mechanisms.

3. **Flexibility vs. Simplicity**: Supporting multiple statement types and complex expressions increases the complexity of the parser but enhances its functionality and usability.

Overall, the `Parser.h` file is a well-designed component of the QuantumLanguage compiler, balancing simplicity, functionality, and performance to ensure robust and reliable parsing of quantum programs.