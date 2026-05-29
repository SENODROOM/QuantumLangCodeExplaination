# `parseImportStmt`

## Purpose
The `parseImportStmt` function is responsible for parsing import statements within the Quantum Language source code. It constructs an `ImportStmt` object that encapsulates the details of the parsed import statement and returns a unique pointer to an `ASTNode`. The function handles both direct imports (`import A as B`) and imports from modules (`from module.sub import A, B`).

## Parameters
- None

## Return Value
- Returns a `std::unique_ptr<ASTNode>` containing the parsed `ImportStmt`.

## Edge Cases
1. **Direct Imports**: When importing directly, there is no base module specified. For example:
   ```quantum
   import A as B
   ```
2. **Imports from Modules**: When importing from a module, the base module is specified before the import keyword. For example:
   ```quantum
   from module.sub import A, B
   ```
3. **Multiple Items**: Multiple items can be imported separated by commas. For example:
   ```quantum
   import A as B, C
   ```
4. **Aliases**: Aliases can be provided for imported items using the `as` keyword. For example:
   ```quantum
   from module.sub import A as B, C as D
   ```

## How It Works
The function begins by determining whether the current token indicates a direct import or an import from a module. If the current token is `TokenType::FROM`, it assumes an import from a module; otherwise, it assumes a direct import.

### Direct Imports
For direct imports:
1. The function reads an identifier which represents the item to be imported.
2. If the next token is `TokenType::AS`, it expects another identifier to serve as the alias for the imported item.
3. The `ImportStmt::Item` structure is populated with the item name and its alias (if any).
4. These items are added to the `imports` vector of the `ImportStmt`.
5. The function continues reading comma-separated items until a non-comma token is encountered.

### Imports from Modules
For imports from modules:
1. The function reads an identifier representing the module name.
2. It then expects the `TokenType::IMPORT` token.
3. Similar to direct imports, it reads comma-separated items, each represented by an identifier.
4. If the next token is `TokenType::AS`, it expects another identifier to serve as the alias for the imported item.
5. The `ImportStmt::Item` structure is populated with the item name and its alias (if any).
6. These items are added to the `imports` vector of the `ImportStmt`.
7. The function continues reading comma-separated items until a non-comma token is encountered.

### Handling Whitespace
After parsing the import statement, the function consumes any trailing newline or semicolon tokens to ensure proper parsing of subsequent statements.

## Interactions With Other Components
The `parseImportStmt` function interacts with several other components of the Quantum Language compiler:
- **Tokenizer**: To retrieve the current token and advance through the input stream.
- **Error Handling**: To report syntax errors when expected tokens are not found.
- **Abstract Syntax Tree (AST)**: To construct an `ImportStmt` object and wrap it in an `ASTNode` for further processing by the compiler.

This function is crucial for correctly interpreting and organizing import statements in the Quantum Language source code, ensuring that the necessary symbols are available for use in the rest of the program.