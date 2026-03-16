# `parseBodyOrStatement`

## Purpose

The `parseBodyOrStatement` function is crucial for handling the parsing of either a single statement or a block in the Quantum Language compiler. This function determines the type of construct based on the next token and processes it appropriately, ensuring that the correct Abstract Syntax Tree (AST) node is returned.

## Parameters

- None

## Return Value

- Returns a unique pointer to an `ASTNode`, which can be either a `BlockStmt` or a `Statement`.

## How It Works

1. **Check for Empty Body**: The function first checks if the next token is a semicolon (`TokenType::SEMICOLON`). If so, it interprets this as an empty body, such as in constructs like `while(cond);` or `for(...);`. In this case:
   - It records the current line number (`ln`) where the semicolon is located.
   - It consumes the semicolon token using `consume()`.
   - It returns a new `ASTNode` containing an empty `BlockStmt` and the recorded line number.

2. **Check for Block Start**: If the next token is not a semicolon but is either an opening brace (`TokenType::LBRACE`) or an indentation marker (`TokenType::INDENT`), the function assumes that a block is starting. In this scenario:
   - It calls `parseBlock()` to parse the entire block of statements.
   - It returns the result of `parseBlock()` as an `ASTNode`.

3. **Parse Single Statement**: For all other cases, where neither a semicolon nor an opening brace/indentation marker is encountered, the function assumes that a single statement is being parsed. In this case:
   - It records the current line number (`ln`) where the statement begins.
   - It initializes a `BlockStmt` object named `block`.
   - It adds the result of `parseStatement()` to the `statements` vector within the `BlockStmt`.
   - Finally, it returns a new `ASTNode` containing the populated `BlockStmt` and the recorded line number.

## Edge Cases

- **Empty Body**: When encountering a semicolon immediately after a control structure (like `while` or `for`), the function correctly identifies it as an empty body and handles it accordingly.
- **Single Statement**: Any other token sequence is treated as a single statement enclosed in a block. This ensures compatibility with various syntactic structures in the Quantum Language.
- **Nested Blocks**: Although not explicitly handled in this snippet, the function's design allows for nested blocks to be parsed recursively by calling itself when encountering an opening brace or indentation.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token for analysis.
- **Error Handling**: While not shown in the snippet, the function should integrate with the error handling mechanisms of the compiler to manage unexpected tokens gracefully.
- **Parsing Context**: The function operates within the broader context of the parser, utilizing shared state and functions like `current()`, `check()`, and `consume()` to navigate through the input stream and build the AST.

This function plays a pivotal role in the Quantum Language compiler by accurately distinguishing between different types of code constructs and building the appropriate AST nodes. Its flexible design ensures robust parsing capabilities across various language features.