# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a critical component of the Quantum Language compiler's parser module, responsible for parsing expressions that may be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement. The primary goal of this function is to handle both simple and complex expressions, including those involving multiple statements connected by commas.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A unique pointer to an ASTNode object representing the parsed expression statement.

## How It Works

1. **Initialization**: The function starts by recording the current line number (`ln`) using `current().line`.

2. **Parsing Expression**: It then calls `parseExpr()` to parse the expression part of the statement. The result is stored in the `expr` variable.

3. **Handling Comma Expressions**:
   - If the next token is a comma (`TokenType::COMMA`), indicating a comma-separated list of expressions, the function enters a loop to process each expression.
   - Inside the loop, a `BlockStmt` is created to hold multiple statements. Each expression is wrapped in an `ExprStmt` and added to the `statements` vector of the `BlockStmt`.
   - The loop continues until a newline, semicolon, or end-of-file is encountered. For each subsequent expression, the function records its line number (`eln`), parses it again, and adds it to the `BlockStmt`.

4. **Consuming Newlines/Semicolons**:
   - After processing all expressions in the comma-separated list, the function consumes any remaining newlines or semicolons using `consume()`. This ensures that the parser moves past these tokens without error.

5. **Returning the Result**:
   - If the original statement was a comma expression, the function returns a unique pointer to the `BlockStmt` containing all the processed expressions.
   - If the original statement was not a comma expression, the function simply returns a unique pointer to an `ExprStmt` containing the single parsed expression.

## Edge Cases

- **Single Expression**: When the input consists of a single expression followed by a semicolon or newline, `parseExprStmt` correctly handles this case by returning a single `ExprStmt`.
  
- **Comma Separated Expressions**: When multiple expressions are separated by commas, `parseExprStmt` processes them as a single statement, wrapping each expression in an `ExprStmt` within a `BlockStmt`.

- **Trailing Commas**: If there are trailing commas after the last expression in the list, `parseExprStmt` will stop processing once it encounters a newline, semicolon, or end-of-file.

- **Empty Input**: If the input stream is empty or only contains whitespace, `parseExprStmt` will return a null pointer, indicating that no valid expression could be parsed.

## Interactions with Other Components

- **Tokenizer**: `parseExprStmt` relies on the tokenizer to provide the sequence of tokens for parsing. The tokenizer should be able to identify different types of tokens such as identifiers, literals, operators, and punctuation marks.

- **Error Handling**: The parser module should have robust error handling mechanisms to manage unexpected tokens or syntax errors gracefully. `parseExprStmt` should integrate with these mechanisms to ensure proper error reporting and recovery.

- **Abstract Syntax Tree Construction**: `parseExprStmt` plays a vital role in constructing the AST. By creating `ExprStmt` nodes for individual expressions and potentially wrapping them in a `BlockStmt`, it facilitates further analysis and code generation phases of the compilation process.

In summary, the `parseExprStmt` function is essential for parsing expression statements in the Quantum Language compiler. Its ability to handle both simple and complex expressions, including those with multiple statements separated by commas, makes it a versatile and important component of the parser module.