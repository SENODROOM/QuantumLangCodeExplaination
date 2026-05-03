# `parseWhileStmt`

`parseWhileStmt` is a function within the Quantum Language compiler's parser responsible for handling the parsing of while statements. This function plays a critical role in interpreting and compiling while loops from the source code into an Abstract Syntax Tree (AST). The AST serves as the foundation for further compilation processes such as code generation or optimization.

## What It Does

The function begins by retrieving the current line number (`ln`) using the `current()` method. It then proceeds to parse the condition of the while loop using the `parseExpr()` function. After parsing the condition, it checks for an optional Python-style colon (`:`) using the `match(TokenType::COLON)` method. If the colon is present, it ensures that it is correctly matched; otherwise, it simply skips over any whitespace using `skipNewlines()`. Following this, the function parses either a single statement or a block of statements as the body of the while loop using the `parseBodyOrStatement()` method. Finally, it constructs a new `ASTNode` containing a `WhileStmt` object with the parsed condition and body, along with the original line number, and returns it.

### Why It Works This Way

This approach allows the parser to handle both traditional and Pythonic styles of writing while loops. By optionally matching a colon after the condition, the function can accommodate different coding conventions without requiring significant changes to the parsing logic. Additionally, the use of `skipNewlines()` ensures that the parser remains robust even when encountering unexpected whitespace, which is common in many programming languages.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed while statement.

## Edge Cases

1. **Missing Colon**: If the source code contains a while loop without a trailing colon, the function will still parse the loop correctly, assuming there is only one statement following the condition.
2. **Empty Body**: If the source code specifies an empty body for the while loop (e.g., `while (condition): pass`), the `parseBodyOrStatement()` method will handle this gracefully, returning an appropriate AST node for an empty block.
3. **Nested Loops**: While not explicitly handled in the provided code snippet, the `parseBodyOrStatement()` method should be capable of dealing with nested structures, allowing for the correct parsing of complex control flow.

## Interactions With Other Components

- **Lexer**: The `current()` method relies on the lexer to provide the next token for parsing.
- **Expression Parser**: The `parseExpr()` method uses the expression parser to interpret the condition of the while loop.
- **Body Parser**: The `parseBodyOrStatement()` method interacts with the body parser to handle the parsing of the loop's body, which can consist of multiple statements or blocks.

By integrating these components seamlessly, `parseWhileStmt` ensures that the quantum language compiler can accurately parse and represent while loops in the source code, facilitating subsequent stages of compilation.