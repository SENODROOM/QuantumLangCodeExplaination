# `parseBodyOrStatement` Function

## Overview
The `parseBodyOrStatement` function is part of the Quantum Language compiler's parser module and is responsible for parsing either a statement body or a single statement. This function is crucial for handling control flow structures like `while` and `for` loops, which can either have an empty body followed by a semicolon or a block of statements enclosed in curly braces `{}` or indented.

## Parameters/Return Value
- **Parameters**: None
- **Return Type**: `std::unique_ptr<ASTNode>`
  - Returns an ASTNode representing either a block of statements (`BlockStmt`) or a single statement.

## Detailed Explanation
### Logic Breakdown
1. **Empty Body Check**:
   ```cpp
   if (check(TokenType::SEMICOLON))
   ```
   If the next token is a semicolon (`;`), it indicates that the loop has an empty body. In such cases:
   - The line number (`ln`) is retrieved from the current token.
   - The semicolon is consumed using `consume()`.
   - An empty `BlockStmt` is created.
   - An ASTNode containing the empty `BlockStmt` and the line number is returned.

2. **Block Body Check**:
   ```cpp
   else if (check(TokenType::LBRACE) || check(TokenType::INDENT))
       return parseBlock();
   ```
   If the next token is either a left brace `{}` or an indentation, it means the loop has a block body. In such cases:
   - The function calls `parseBlock()` to handle the parsing of the block of statements.
   - The result of `parseBlock()` is returned as an ASTNode.

3. **Single Statement Body**:
   ```cpp
   else
   {
       int ln = current().line;
       BlockStmt block;
       block.statements.push_back(parseStatement());
       return std::make_unique<ASTNode>(std::move(block), ln);
   }
   ```
   If neither a semicolon nor a block body indicator is found, it implies that the loop has only one statement. In such cases:
   - The line number (`ln`) is retrieved from the current token.
   - A new `BlockStmt` is created.
   - The function calls `parseStatement()` to parse the single statement and adds it to the `statements` vector of the `BlockStmt`.
   - An ASTNode containing the `BlockStmt` and the line number is returned.

### Edge Cases
- **Empty Body**: When encountering a `while` or `for` loop followed immediately by a semicolon, the function correctly identifies and handles the empty body case.
- **Block Body**: When encountering a `while` or `for` loop with a block of statements enclosed in curly braces `{}` or indented, the function correctly parses the block body.
- **Single Statement**: When encountering a `while` or `for` loop with a single statement following it, the function correctly parses the single statement within a block context.

### Interactions with Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve the current token and check its type.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely interacts with error handling mechanisms to report syntax errors if unexpected tokens are encountered.
- **Control Flow Parsing**: This function is integral to parsing control flow constructs. It ensures that the correct structure is parsed based on the presence of a semicolon or a block body indicator.
- **ASTNode Creation**: The function creates instances of `ASTNode` to represent the parsed control flow structure, which are then used by higher-level components of the compiler for further processing.

In summary, the `parseBodyOrStatement` function is essential for accurately parsing control flow structures in the Quantum Language compiler. Its design allows it to handle various scenarios, including empty bodies, block bodies, and single statements, ensuring robust parsing capabilities.