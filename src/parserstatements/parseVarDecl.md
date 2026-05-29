# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is designed to handle the parsing of variable declarations. This function supports both single and multiple variable declarations, allowing for optional type hints and initializers. The primary goal is to accurately convert the source code into an Abstract Syntax Tree (AST) that represents the structure of the program.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet; however, it implicitly relies on global state managed by the parser, such as the current token being processed.
  
- **Return Value**:
  - An `ASTNodePtr`, which is a smart pointer to an `ASTNode`. This node represents either a single or multiple variable declarations within the quantum language context.

## Why It Works This Way

The function operates by first identifying whether the declaration is constant (`const`) or not based on the preceding keyword. It then proceeds to parse the variable names and optionally their types and initializers. For multi-variable declarations, it enters a loop where each subsequent variable is parsed until a comma is encountered followed by a newline or semicolon, indicating the end of the declaration block.

### Detailed Breakdown:

1. **Initialization**:
   ```cpp
   int ln = current().line;
   ```
   Stores the line number of the current token to maintain accurate error reporting and AST node placement.

2. **Variable Name Parsing**:
   ```cpp
   std::string name;
   if (check(TokenType::IDENTIFIER))
       name = consume().value;
   else if (isCTypeKeyword(current().type))
       name = consume().value;
   else
       throw ParseError("Expected variable name (got '" + current().value + "')", current().line, current().col);
   ```
   Checks if the next token is an identifier or a C-type keyword (like `int`, `float`). If so, it consumes the token and assigns its value to `name`. Otherwise, it throws a `ParseError`.

3. **Type Hint Parsing**:
   ```cpp
   std::string typeHint;
   if (check(TokenType::COLON))
   {
       consume(); // eat :
       if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type))
           typeHint = consume().value;
   }
   ```
   If a colon (`:`) is found after the variable name, it indicates a type hint. The function consumes the colon and checks if the next token is an identifier or a C-type keyword. If so, it consumes the token and assigns its value to `typeHint`.

4. **Initializer Parsing**:
   ```cpp
   ASTNodePtr init;
   if (match(TokenType::ASSIGN))
       init = parseExpr();
   ```
   If an assignment operator (`=`) is found immediately after the variable name, the function parses the expression following it as the initializer for the variable.

5. **Multi-Variable Declaration Handling**:
   ```cpp
   if (check(TokenType::COMMA))
   {
       auto block = std::make_unique<ASTNode>(BlockStmt{}, ln);
       block->as<BlockStmt>().statements.push_back(
           std::make_unique<ASTNode>(VarDecl{isConst, name, std::move(init), typeHint}, ln));
       while (match(TokenType::COMMA))
       {
           skipNewlines();
           if (atEnd() || check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
               break;
           std::string n2;
           if (check(TokenType::IDENTIFIER))
               n2 = consume().value;
           else if (isCTypeKeyword(current().type))
               n2 = consume().value;
           else
               break;
           
           std::string h2;
           if (check(TokenType::COLON))
           {
               consume();
               if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type))
                   h2 = consume().value;
           }

           ASTNodePtr init2;
           if (match(TokenType::ASSIGN))
               init2 = parseExpr();
           block->as<BlockStmt>().statements.push_back(
               std::make_unique<ASTNode>(VarDecl{isConst, n2, std::move(init2), h2}, ln));
       }
       while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
           consume();
   ```
   If a comma is detected, the function enters a loop to handle multiple variables. Each iteration of the loop processes another variable, including its name, type hint, and initializer. The loop continues until a comma is followed by a newline or semicolon, signaling the end of the declaration block.

6. **Final Consumption**:
   ```cpp
   while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
       consume();
   ```
   Consumes any remaining newlines or semicolons to ensure proper parsing of the statement block.

## Edge Cases

- **Single Variable Declaration**: Handles cases where only one variable is declared without additional commas.
- **Multiple Variables Declaration**: Correctly parses sequences of variable declarations separated by commas, ensuring they are grouped together in the AST.
- **Missing Type Hint**: Allows for variable declarations without a specified type hint.
- **Missing Initializer**: Supports variable declarations without initializers.
- **Unexpected Token**: Throws a `ParseError` when encountering unexpected tokens during parsing.

## Interactions with Other Components

- **Tokenizer**: The function depends on the tokenizer to provide the sequence of tokens for parsing.
- **Error Reporting**: Utilizes the error reporting mechanism to handle syntax errors gracefully.
- **AST Construction**: Builds an AST by creating nodes representing variable declarations and adding them to the appropriate parent node.

This comprehensive approach ensures robust parsing of variable