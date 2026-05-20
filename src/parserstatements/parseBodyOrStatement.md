# `parseBodyOrStatement` Function

## Overview
The `parseBodyOrStatement` function is an essential part of the Quantum Language compiler's parser module. It evaluates the next token in the source code to decide whether the following content represents the start of a statement body or a single statement. This function plays a pivotal role in constructing the Abstract Syntax Tree (AST) accurately.

## Parameters and Return Value
- **Parameters**: None
- **Return Type**: `std::unique_ptr<ASTNode>`
  - Returns an ASTNode representing either a block statement or a single statement.

## How It Works
1. **Check for Empty Body**:
   - The function first checks if the next token is a semicolon (`TokenType::SEMICOLON`). If so, it consumes the semicolon and returns an ASTNode containing an empty block statement (`BlockStmt{}`). This scenario typically occurs in control structures like `while` and `for` loops where the body might be intentionally left empty.
   
2. **Check for Block Start**:
   - If the next token is not a semicolon but rather an opening brace (`TokenType::LBRACE`) or an indentation (`TokenType::INDENT`), the function proceeds to parse a block using the `parseBlock()` method. A block in quantum language is defined as a sequence of statements enclosed within curly braces `{}` or indented blocks.
   
3. **Single Statement**:
   - If neither a semicolon nor an opening brace/indentation is found, the function assumes that the next token begins a single statement. It initializes a `BlockStmt` object and adds the parsed statement to its `statements` vector. Finally, it returns an ASTNode containing this block statement.

## Edge Cases
- **Empty Body**: When encountering a semicolon immediately after a control structure, indicating an intentionally empty body.
- **Block Structure**: Handling both explicit `{}` blocks and implicitly defined indented blocks.
- **Single Statement**: Parsing scenarios where only one statement exists without any enclosing block.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the current token being processed.
- **Error Handling**: While not explicitly shown in the provided snippet, this function likely integrates with error handling mechanisms to manage unexpected tokens or syntax errors gracefully.
- **AST Construction**: This function directly contributes to building the AST by creating nodes for different types of statements and blocks.

This comprehensive approach ensures that the parser can correctly interpret various syntactic constructs in the Quantum Language, facilitating accurate compilation and execution of the code.