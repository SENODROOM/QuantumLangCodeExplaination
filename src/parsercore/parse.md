# `parse` Function

## Overview
The `parse` function is a fundamental method in the Quantum Language compiler's parser module. Its main objective is to convert a series of lexical tokens into an Abstract Syntax Tree (AST), which encapsulates the syntactic structure of the quantum program. The function iterates through the tokens, constructing the AST by parsing individual statements and handling newlines appropriately.

### Parameters
- **None**: The `parse` function does not accept any parameters explicitly. It operates on the global token stream managed by the parser.

### Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to the root node of the constructed AST. This root node represents a block statement (`BlockStmt`) containing all parsed statements.

### Edge Cases
1. **Empty Input**: If the input token stream is empty or contains only whitespace, the function will return an empty block statement.
2. **Syntax Errors**: If the input contains syntax errors that cannot be resolved, the function may throw an exception or handle the error gracefully based on the compiler's design.
3. **Comments**: The function should ignore comments within the token stream as they do not contribute to the syntactic structure of the program.

### Interactions with Other Components
- **Lexical Analyzer**: The `parse` function relies on the lexical analyzer to provide a sequence of tokens. These tokens represent the basic building blocks of the quantum language.
- **Token Stream Management**: The function manages the global token stream, advancing through it as it parses each statement.
- **Error Handling**: Depending on the implementation, the `parse` function may interact with error-handling mechanisms to report issues encountered during parsing.

## Detailed Explanation
The `parse` function begins by creating a unique pointer to an `ASTNode` representing a block statement. This block node serves as the container for all parsed statements.

```cpp
auto block = std::make_unique<ASTNode>(BlockStmt{}, 0);
auto &stmts = block->as<BlockStmt>().statements;
```

Next, the function skips any leading newlines in the token stream using the `skipNewlines()` method. This ensures that the first statement is correctly identified even if there are initial blank lines.

```cpp
skipNewlines();
```

The core loop of the `parse` function continues until the end of the token stream is reached (`!atEnd()`). Within this loop, the function calls `parseStatement()` to parse each individual statement and adds it to the list of statements in the block node.

```cpp
while (!atEnd())
{
    stmts.push_back(parseStatement());
    skipNewlines();
}
```

After all statements have been parsed, the function returns the unique pointer to the block node, which now contains the entire AST of the quantum program.

```cpp
return block;
```

### Why It Works This Way
This approach allows the `parse` function to efficiently construct the AST by breaking down the task into smaller, manageable steps. By iterating through the token stream and parsing each statement individually, the function can handle complex programs with multiple statements without becoming overly cumbersome. Additionally, skipping newlines after parsing each statement helps maintain the correct order and spacing of elements in the AST, ensuring that the syntactic structure accurately reflects the original source code.