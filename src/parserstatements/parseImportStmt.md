# `parseImportStmt`

## Purpose

The `parseImportStmt` function parses import statements in the Quantum Language source code. It constructs an `ImportStmt` object representing the parsed import statement and returns a unique pointer to an `ASTNode` containing this statement.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - Returns a unique pointer (`std::unique_ptr`) to an `ASTNode`, which contains an `ImportStmt` object. The `ImportStmt` object holds information about the imported items and their aliases.

## How It Works

1. **Initialization**: 
   - The function initializes an `ImportStmt` object named `stmt`.
   - It retrieves the current line number (`ln`) using `current().line`.

2. **Parsing Based on Statement Type**:
   - If the current token indicates a `from` statement (`isFrom` is true), it processes the import statement in the form of `from module.sub import A, B`. However, currently, it only reads an identifier (e.g., `module.sub`).
     - The function expects a `TokenType::IDENTIFIER` (the module name) and then checks for the `TokenType::IMPORT` keyword.
     - It enters a loop where it repeatedly expects identifiers (item names to be imported) and optionally aliases (using the `as` keyword). Each item is stored in an `ImportStmt::Item` structure within the `stmt.imports` vector.
     - The loop continues until a comma is encountered, indicating another item to import.
   - If the current token indicates a direct import statement (`isFrom` is false), it processes the import statement in the form of `import A as B, C`.
     - The function sets the `module` field of `stmt` to an empty string since there's no base module being specified.
     - It enters a similar loop to the previous case but without expecting a module name initially.

3. **Consuming Whitespace**:
   - After parsing the import items, the function consumes any trailing whitespace or newlines using a loop that checks for either `TokenType::NEWLINE` or `TokenType::SEMICOLON`.

4. **Returning AST Node**:
   - Finally, the function creates an `ASTNode` containing the parsed `ImportStmt` object and its line number (`ln`). It returns a unique pointer to this `ASTNode`.

## Edge Cases

- **Empty Module Name**: If the `from` statement does not specify a module name, the current implementation treats it as an error and expects a module name. This can be improved to handle cases where direct imports are allowed without specifying a module.
  
- **Missing Aliases**: The function allows importing items without specifying aliases. In such cases, the `alias` field in the `ImportStmt::Item` will remain empty.

- **Trailing Commas**: The function handles trailing commas gracefully by continuing to consume tokens until a newline or semicolon is encountered.

## Interactions With Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve the current token and check subsequent tokens. It relies on the tokenizer to identify different types of tokens like `IDENTIFIER`, `IMPORT`, `AS`, `COMMA`, etc.

- **Error Handling**: The function includes basic error handling to ensure that expected tokens are present. For example, it expects an `IDENTIFIER` after `'from'` and `'import'` keywords, and an `IDENTIFIER` after `'as'`.

- **Abstract Syntax Tree (AST)**: The function constructs an `ImportStmt` object and wraps it in an `ASTNode`. This `ASTNode` is later used by other parts of the compiler to generate code, perform semantic analysis, or optimize the program.

This function is crucial for correctly interpreting and processing import statements in the Quantum Language, ensuring that the abstract syntax tree accurately reflects the intended usage of imported modules and items.