# Parser.h

## Overview

`Parser.h` is a header file for the parser component of the Quantum Language compiler. This file defines the `Parser` class responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). The parser follows a top-down approach, utilizing various parsing functions to handle different types of statements and expressions in the language.

## Design Decisions

### Error Handling

The `ParseError` class extends `std::runtime_error` to provide additional context about errors during parsing, such as the line and column numbers where the error occurred. This decision was made to enhance debugging capabilities and provide more precise error messages to the user.

### Pratt Parsing Algorithm

The parser uses the Pratt parsing algorithm for expression evaluation. This algorithm allows for flexible handling of operator precedence and associativity without requiring complex recursive descent parsers. It simplifies the implementation and improves performance.

### Modular Parsing Functions

Each major type of construct (e.g., variable declarations, function definitions, control structures) has its own parsing function. This modular approach makes the parser easier to understand, maintain, and extend.

## Classes and Functions

### ParseError

**Purpose:** Custom exception class for parsing errors.

**Behavior:** Inherits from `std::runtime_error` and adds line and column information.

```cpp
class ParseError : public std::runtime_error
{
public:
    int line, col;
    ParseError(const std::string &msg, int l, int c);
};
```

### Parser

**Purpose:** Main parser class that converts tokens into an AST.

**Behavior:** Takes a vector of tokens and provides a method to parse them into an AST.

```cpp
class Parser
{
public:
    explicit Parser(std::vector<Token> tokens);
    ASTNodePtr parse();

private:
    std::vector<Token> tokens;
    size_t pos;

    // Token helpers
    Token &current();
    Token &peek(int offset = 1);
    Token &consume();
    Token &expect(TokenType t, const std::string &msg);
    bool check(TokenType t) const;
    bool match(TokenType t);
    bool atEnd() const;
    void skipNewlines();

    // Parsing methods
    ASTNodePtr parseStatement();
    ASTNodePtr parseBlock();
    ASTNodePtr parseBodyOrStatement(); // block OR single statement (brace-optional)
    ASTNodePtr parseVarDecl(bool isConst);
    ASTNodePtr parseFunctionDecl();
    ASTNodePtr parseClassDecl();
    ASTNodePtr parseIfStmt();
    ASTNodePtr parseWhileStmt();
    ASTNodePtr parseForStmt();
    ASTNodePtr parseReturnStmt();
    ASTNodePtr parsePrintStmt();
    ASTNodePtr parseInputStmt();
    ASTNodePtr parseCoutStmt(); // cout << x << y << endl
    ASTNodePtr parseCinStmt();  // cin >> x >> y
    ASTNodePtr parseImportStmt(bool isFrom = false);
    ASTNodePtr parseExprStmt();
    ASTNodePtr parseCTypeVarDecl(const std::string &typeHint); // int x = ...  / int* p = ...
    bool isCTypeKeyword(TokenType t) const;

    // Expression parsing (Pratt-style precedence)
    ASTNodePtr parseExpr();
    ASTNodePtr parseAssignment();
    ASTNodePtr parseOr();
    ASTNodePtr parseAnd();
    ASTNodePtr parseBitwise();
    ASTNodePtr parseEquality();
    ASTNodePtr parseComparison();
    ASTNodePtr parseShift();
    ASTNodePtr parseAddSub();
    ASTNodePtr parseMulDiv();
    ASTNodePtr parsePower();
    ASTNodePtr parseUnary();
    ASTNodePtr parsePostfix();
    ASTNodePtr parsePrimary();

    ASTNodePtr parseArrayLiteral();
    ASTNodePtr parseDictLiteral();
    ASTNodePtr parseLambda();
    ASTNodePtr parseArrowFunction(std::vector<std::string> params, int ln);
    std::vector<ASTNodePtr> parseArgList();
    // Returns param names; populates outIsRef with true for each & (reference) param
    std::vector<std::string> parseParamList(std::vector<bool> *outIsRef = nullptr, std::vector<ASTNodePtr> *outDefaultArgs = nullptr, std::vector<std::string> *outParamTypes = nullptr);
};
```

## Tradeoffs and Limitations

- **Flexibility vs. Complexity:** While the Pratt parsing algorithm offers flexibility, it can also lead to more complex implementations compared to traditional recursive descent parsers.
- **Error Reporting:** Providing detailed error reporting requires careful tracking of token positions, which can add overhead to the parsing process.
- **Language Features:** The current implementation focuses on basic constructs. Extending the parser to support advanced features may require significant changes to existing code.

This README.md provides a comprehensive overview of the `Parser.h` file, including its role in the compiler pipeline, key design decisions, and documentation of major classes/functions.