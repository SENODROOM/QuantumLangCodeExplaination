# `parseImportStmt`

## Purpose

The `parseImportStmt` function is responsible for parsing import statements in the Quantum Language source code. It constructs an `ImportStmt` object that represents the parsed import statement and returns a unique pointer to an `ASTNode` containing this statement.

## How It Works

### Parsing Structure

1. **Module Specification**:
   - If the current token indicates a `from` statement (`isFrom` is true):
     - The function expects an identifier representing the module name (e.g., `module.sub`).
     - After the module name, it expects the keyword `import`.
   - If not a `from` statement (`isFrom` is false):
     - The function assumes direct imports without a base module.

2. **Import Items**:
   - The function enters a loop where it repeatedly parses individual import items until it encounters a token that is neither a comma nor a newline/semicolon.
   - For each import item:
     - It expects an identifier representing the item's name (e.g., `A`, `B`).
     - Optionally, it checks for the `as` keyword followed by another identifier to specify an alias for the imported item.

3. **Whitespace Handling**:
   - After parsing the import statement, the function consumes any trailing newlines or semicolons to ensure proper parsing of subsequent statements.

4. **Return Value**:
   - The function returns a unique pointer to an `ASTNode` containing the parsed `ImportStmt`. The node includes the line number where the statement was found.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet. However, based on context and typical usage in parsers, this function likely takes a reference to the parser state, which includes tokens and their positions.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` containing the parsed `ImportStmt`.

## Edge Cases

1. **Invalid Syntax**:
   - If the syntax deviates from expected patterns (e.g., missing `import` keyword after `from`), the function will throw exceptions indicating the expected token.

2. **Empty Module Name**:
   - When parsing direct imports, the function sets the module name to an empty string (`""`). This implies that the import is happening at the global level rather than within a specific module.

3. **Trailing Commas**:
   - The function handles trailing commas gracefully by continuing to parse import items until it encounters a non-comma token.

## Interactions With Other Components

1. **Token Stream**:
   - The function relies on the token stream managed by the parser. It uses methods like `current()`, `expect()`, and `match()` to navigate through the tokens.

2. **Abstract Syntax Tree (AST)**:
   - Upon successful parsing, the function creates an `ImportStmt` object and encapsulates it within an `ASTNode`. This AST node is then returned to be integrated into the overall AST structure of the program.

3. **Error Handling**:
   - The function incorporates error handling mechanisms to manage unexpected tokens or syntax errors. Exceptions are thrown when the expected tokens are not encountered, ensuring robustness during parsing.

4. **Line Number Tracking**:
   - The function captures the line number of the import statement using `current().line` and stores it in the `ln` variable. This information is crucial for debugging and error reporting purposes.

This comprehensive approach ensures that the `parseImportStmt` function can accurately parse various forms of import statements in the Quantum Language, providing a solid foundation for further compilation steps.