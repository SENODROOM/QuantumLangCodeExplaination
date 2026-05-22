# `parseImportStmt`

## Purpose
The `parseImportStmt` function is responsible for parsing import statements within the Quantum Language source code. This function constructs an `ImportStmt` object that encapsulates the details of the parsed import statement and returns a unique pointer to an `ASTNode` containing this object.

## Parameters
- None explicitly listed in the provided code snippet; however, it relies on global state or context variables such as `current()`, `expect()`, `match()`, and `consume()` which are not shown here.

## Return Value
- Returns a `unique_ptr<ASTNode>` containing the parsed `ImportStmt`.

## Detailed Explanation
1. **Initialization**:
   - The function starts by initializing the line number (`ln`) where the import statement begins using `current().line`.
   - An empty `ImportStmt` object named `stmt` is created to store the parsed information.

2. **Parsing Logic**:
   - If the `isFrom` flag is true, it indicates that the import statement follows the format `from <module> import <item>, <item>...`. In this case:
     - The function expects a `TokenType::IDENTIFIER` token to identify the module name. If found, it stores the module name in `stmt.module`.
     - Another `TokenType::IMPORT` token is expected immediately after the module name. If not found, an error message is thrown.
     - The function then enters a loop to parse each imported item. Inside the loop:
       - An `ImportStmt::Item` object is initialized.
       - The function expects another `TokenType::IDENTIFIER` token to identify the item being imported. If found, it stores the item name in `item.name`.
       - If the next token matches `TokenType::AS`, indicating an alias is defined, the function expects another `TokenType::IDENTIFIER` token to define the alias. If found, it stores the alias in `item.alias`.
       - Each parsed item is added to `stmt.imports`.
       - The loop continues until a `TokenType::COMMA` token is encountered, signaling the end of the list of items to be imported.
   - If the `isFrom` flag is false, it indicates that the import statement follows the format `import <item>, <item>...`. In this case:
     - The function sets `stmt.module` to an empty string since there is no base module specified.
     - Similar to the previous case, the function enters a loop to parse each imported item. Inside the loop:
       - Steps 3a to 3c are repeated to parse each item and its optional alias.
       - The loop continues until a `TokenType::COMMA` token is encountered.

3. **Handling Newlines and Semicolons**:
   - After parsing all items, the function checks for any trailing newlines (`TokenType::NEWLINE`) or semicolons (`TokenType::SEMICOLON`). If either is found, it consumes them using `consume()` to ensure proper parsing of subsequent statements.

4. **Returning the Result**:
   - Finally, the function creates a unique pointer to an `ASTNode` containing the parsed `ImportStmt` and the original line number (`ln`). This node is returned to the caller.

## Edge Cases
- **Missing Tokens**: If any required tokens (e.g., `TokenType::IDENTIFIER`, `TokenType::IMPORT`, etc.) are missing, the function throws an error message indicating the expected token was not found.
- **Trailing Commas**: While the parser handles multiple items separated by commas, it may need additional logic to handle trailing commas gracefully without throwing errors.
- **Complex Module Names**: Although the current implementation only reads simple identifiers, it could be extended to support more complex module names containing dots (e.g., `module.sub`).

## Interactions with Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve tokens from the input source code. It relies on functions like `current()`, `expect()`, and `match()` to interact with the tokenizer.
- **Abstract Syntax Tree (AST)**: The parsed `ImportStmt` is stored in an `ASTNode`, which is part of the AST structure used to represent the syntactic elements of the source code. This interaction allows for further processing and analysis of the import statement during compilation.

This function plays a crucial role in handling import statements, ensuring they are correctly parsed and represented in the AST, which facilitates subsequent stages of the compilation process.