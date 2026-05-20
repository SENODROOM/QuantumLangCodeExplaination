# `parseImportStmt`

## Purpose
The `parseImportStmt` function is designed to parse import statements in the Quantum Language source code. It constructs an `ImportStmt` object representing the parsed import statement and returns a unique pointer to an ASTNode containing this object.

## Parameters
- None explicitly listed; however, implicit parameters include:
  - The parser's internal state, such as the current token being processed (`current()`).
  - The type of import statement being parsed (`isFrom`).

## Return Value
- Returns a unique pointer to an `ASTNode` object.
  - The `ASTNode` contains an `ImportStmt` object.
  - The line number where the import statement was found (`ln`).

## How It Works
1. **Initialization**:
   - The function starts by retrieving the current line number (`ln`) using `current().line`.
   - An `ImportStmt` object named `stmt` is initialized to store the parsed information.

2. **Parsing Based on Statement Type**:
   - If the statement is of the form `from ... import ...`, indicated by `isFrom` being true:
     - The function expects an `IDENTIFIER` token to represent the module name.
     - After the module name, it expects an `IMPORT` token.
     - It then enters a loop to parse individual items being imported:
       - For each item, it expects an `IDENTIFIER` token representing the item name.
       - Optionally, it checks for an `AS` keyword followed by another `IDENTIFIER` token to define an alias for the item.
       - Each parsed item is added to the `imports` vector of the `ImportStmt` object.
   - If the statement is of the form `import ...`, indicated by `isFrom` being false:
     - The module field of the `ImportStmt` object is left empty since there is no base module.
     - The function proceeds similarly to the `from ... import ...` case, but without expecting a module name before the import list.

3. **Handling Whitespace**:
   - The function consumes any trailing whitespace or newlines (`NEWLINE` or `SEMICOLON`) until it reaches valid syntax.

4. **Creating and Returning the ASTNode**:
   - An `ASTNode` is created with the `ImportStmt` object and the line number.
   - This `ASTNode` is wrapped in a unique pointer and returned.

## Edge Cases
- **Invalid Syntax**: If the input does not conform to expected import statement syntax (e.g., missing keywords, incorrect token types), the function will throw an error indicating the expected token.
- **Empty Module Name**: When parsing `import ...`, the module field remains empty, which can be interpreted as importing directly from the global namespace.
- **Multiple Aliases**: Multiple aliases can be defined for different items in the same import statement, separated by commas.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing.
- **Error Handling**: Errors during parsing are handled through calls to functions like `expect` and `consume`, which manage the parser's state and report errors appropriately.
- **Abstract Syntax Tree (AST)**: The parsed `ImportStmt` object is used to create an `ASTNode`, which is part of the broader AST structure constructed by the parser. This node can then be used for further semantic analysis or code generation.