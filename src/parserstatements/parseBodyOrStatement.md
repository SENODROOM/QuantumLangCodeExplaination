# `parseBodyOrStatement` Function

## Overview
The `parseBodyOrStatement` function is an essential component of the Quantum Language compiler's parser module. Its primary role is to determine whether the next token in the source code represents the beginning of a statement body or a standalone statement. The function returns an ASTNode containing either a BlockStmt or a Statement depending on the parsed input.

### Why It Works This Way
This design allows for flexible parsing of control flow structures such as `while` and `for`. By checking for semicolons (`TokenType::SEMICOLON`) and curly braces (`TokenType::LBRACE`), the function can distinguish between an empty body followed by a semicolon and a block of statements enclosed within curly braces. Additionally, the function handles indentation using `TokenType::INDENT`, which is common in some programming languages for defining blocks.

## Parameters/Return Value
- **Parameters**: None
- **Return Type**: `std::unique_ptr<ASTNode>`
  - Returns an ASTNode that encapsulates either a BlockStmt or a Statement.

## Edge Cases
1. **Empty Body Followed by Semicolon**:
   ```cpp
   while (condition);
   ```
   In this case, the function will consume the semicolon and return an ASTNode with an empty BlockStmt.

2. **Single Statement**:
   ```cpp
   while (condition)
       statement;
   ```
   Here, the function will parse the statement and wrap it inside a BlockStmt before returning it.

3. **Block of Statements**:
   ```cpp
   while (condition) {
       statement1;
       statement2;
   }
   ```
   For this scenario, the function will call `parseBlock()` to handle the entire block of statements.

4. **Indentation**:
   ```cpp
   while (condition)
       statement;  // Assuming 'statement' uses indentation
   ```
   If the language supports indentation for block delimiters, the function will recognize the indented line as part of the same block.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the next token for analysis.
- **Error Handling**: If unexpected tokens are encountered during parsing, appropriate error messages are generated to aid in debugging.
- **Control Flow Parsing**: `parseBodyOrStatement` interacts closely with functions responsible for parsing different types of control flow statements, ensuring proper nesting and execution order.

By handling both statement bodies and individual statements, `parseBodyOrStatement` facilitates the accurate parsing of complex control flow constructs in the Quantum Language, making it a vital function for maintaining the integrity and correctness of the compiled code.