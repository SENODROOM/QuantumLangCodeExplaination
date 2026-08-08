# `parseImportStmt`

## Purpose
The `parseImportStmt` function is designed to parse import statements in the Quantum Language source code. It constructs an `ImportStmt` object containing the details of the parsed import statement and returns a unique pointer to an `ASTNode`. This function supports both direct imports (`import A as B, C`) and imports from modules (`from module.sub import A, B`).

## Parameters
- None

## Return Value
- Returns a `std::unique_ptr<ASTNode>` containing the parsed `ImportStmt` object and the line number where the import statement was found.

## How It Works
1. **Initialization**: The function starts by retrieving the current line number using `current().line` and initializing an `ImportStmt` object named `stmt`.
2. **Parsing Imports**:
   - If the import statement begins with `from`, it reads the module name followed by `import`. Then, it enters a loop to parse each imported item.
     - For each item, it expects an identifier representing the item's name.
     - Optionally, it checks for an `as` keyword to define an alias for the imported item.
   - If the import statement does not begin with `from`, it assumes the items are being imported directly without a base module. The module name remains empty, and the loop parses each item similarly to the `from` case.
3. **Handling Newlines and Semicolons**: After parsing all the import items, the function consumes any trailing newlines or semicolons until it reaches a non-whitespace token.
4. **Returning the AST Node**: Finally, it creates an `ASTNode` containing the parsed `ImportStmt` object and the line number, then returns a unique pointer to this node.

## Edge Cases
- **Empty Module Name**: When importing directly, the module name is set to an empty string.
- **Multiple Aliases**: Multiple aliases can be defined for imported items using the `as` keyword.
- **Trailing Commas**: Trailing commas after the last imported item are allowed and ignored.

## Interactions With Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve tokens such as identifiers, keywords (`from`, `import`, `as`), and punctuation (`comma`, `semicolon`, `newline`). These tokens are essential for correctly identifying and parsing the import statement structure.
- **Error Handling**: The function includes error handling mechanisms to ensure that required tokens are present in the correct order. If expected tokens are missing, appropriate error messages are generated.
- **Abstract Syntax Tree (AST)**: The parsed `ImportStmt` object is used to construct an `ASTNode`, which represents the import statement in the abstract syntax tree. This allows for further processing and analysis of the import statement during compilation.

This comprehensive approach ensures that the `parseImportStmt` function accurately handles various import statement scenarios in the Quantum Language, providing robust support for the language's syntax and semantics.