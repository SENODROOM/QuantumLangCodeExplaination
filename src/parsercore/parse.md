# `parse` Function

## Overview
The `parse` function acts as the primary interface for parsing quantum programs within the Quantum Language compiler. Its role is to convert a series of lexical tokens into an Abstract Syntax Tree (AST). The AST effectively captures the syntactic structure of the quantum code, facilitating subsequent compilation and execution phases.

### Why It Works This Way
The design of the `parse` function follows a top-down approach where it constructs the AST by recursively parsing individual statements. By skipping newlines at the beginning and end of each statement, the function ensures that the AST accurately reflects the intended structure of the program, even when there are extraneous whitespace characters.

### Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed quantum program. The node's type is `BlockStmt`, indicating that it contains multiple statements organized in a block.

### Edge Cases
1. **Empty Input**: If the input stream is empty or consists solely of whitespace, the function will return an empty `BlockStmt`.
2. **Syntax Errors**: If the input contains syntax errors, such as missing keywords or misplaced operators, the function may throw exceptions or produce an incomplete AST.
3. **Comments**: The function should handle comments appropriately, either ignoring them or incorporating them into the AST as special nodes.

### Interactions With Other Components
- **Lexer**: Before calling `parse`, the lexer must have tokenized the input source code. These tokens are consumed by `parse` to construct the AST.
- **Error Handling**: During parsing, error handling mechanisms are invoked to manage unexpected tokens or malformed statements. These mechanisms might include logging errors or throwing exceptions.
- **Symbol Table**: As parsing progresses, symbols (e.g., qubits, gates) are added to a symbol table to support semantic analysis and validation during later stages of compilation.

Here is a more detailed breakdown of the implementation:

```cpp
auto block = std::make_unique<ASTNode>(BlockStmt{}, 0); // Create a root ASTNode for BlockStmt
auto &stmts = block->as<BlockStmt>().statements;      // Reference to the list of statements in the block

skipNewlines(); // Skip any leading newlines

while (!atEnd()) // Continue parsing until the end of the input is reached
{
    stmts.push_back(parseStatement()); // Parse a single statement and add it to the block's list
    skipNewlines(); // Skip any trailing newlines after the statement
}

return block; // Return the fully constructed ASTNode representing the quantum program
```

This implementation ensures that the quantum program is parsed correctly, maintaining its structure and allowing for further processing in the compiler pipeline.