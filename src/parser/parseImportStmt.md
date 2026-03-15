# `parseImportStmt`

## Purpose

The `parseImportStmt` function is designed to parse import statements in the Quantum Language source code. It handles both direct import statements (`import A, B`) and import statements with a specified base module (`from module.sub import A, B`). The parsed information is encapsulated into an `ImportStmt` object, which is then converted into an `ASTNode` and returned.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the function signature, but it relies on global state such as `current()` and `expect()`.
  
- **Return Value**:
  - Returns a unique pointer to an `ASTNode` containing the parsed `ImportStmt`.

## How It Works

1. **Initialization**:
   - The function starts by retrieving the current line number using `current().line`.
   - An `ImportStmt` object named `stmt` is initialized to store the parsed import statement details.

2. **Parsing Based on Statement Type**:
   - If the current token indicates a `from` statement (`isFrom` is true):
     - It expects an `IDENTIFIER` token representing the module name (e.g., `module.sub`).
     - It then expects an `IMPORT` token to confirm the start of the import list.
     - Inside a loop that continues until a comma or end of input is encountered:
       - For each item to be imported, it expects an `IDENTIFIER` token representing the item's name.
       - Optionally, it checks if the next token is `AS`, indicating an alias should be assigned to the imported item. If so, it expects another `IDENTIFIER` token for the alias.
       - Each parsed item is added to the `imports` vector within the `ImportStmt` object.
   
   - If the current token indicates a direct import statement (`isFrom` is false):
     - It initializes the `module` field of the `ImportStmt` object to an empty string, signifying that no base module is specified.
     - Similar to the `from` statement case, it enters a loop to parse each item to be imported:
       - It expects an `IDENTIFIER` token representing the item's name.
       - Optionally, it checks for an `AS` token to assign an alias to the imported item.
       - Each parsed item is added to the `imports` vector.

3. **Handling Newlines and Semicolons**:
   - After parsing the import statement, the function consumes any trailing newlines (`NEWLINE`) or semicolons (`SEMICOLON`) to ensure proper handling of the syntax.

4. **Creating ASTNode**:
   - Finally, the function returns a unique pointer to an `ASTNode` constructed with the parsed `ImportStmt` and the original line number.

## Edge Cases

- **Empty Module Name**: If the `from` statement does not specify a module name, the `module` field will remain an empty string.
- **Missing Alias**: If an `AS` keyword is used without providing an alias, the `alias` field for that item will also remain an empty string.
- **Syntax Errors**: The function includes error checking to ensure that the expected tokens (`IDENTIFIER`, `IMPORT`, `AS`) are present at appropriate places. If these tokens are missing, it throws an exception with a descriptive message.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. Functions like `current()`, `expect()`, and `match()` interact with the tokenizer to retrieve and validate tokens.
- **AST Construction**: Once the `ImportStmt` is fully parsed, the function constructs an `ASTNode` using the parsed data. This node can then be further processed by other parts of the compiler to generate intermediate representation or compile the code.

By following this structured approach, `parseImportStmt` ensures that import statements are correctly parsed and represented in the Abstract Syntax Tree (AST), facilitating subsequent stages of compilation.