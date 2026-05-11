# `parseImportStmt`

## Purpose

The `parseImportStmt` function is responsible for parsing import statements within the Quantum Language source code. Its primary goal is to construct an `ImportStmt` object that accurately represents the parsed import statement and then return a unique pointer to an `ASTNode` containing this statement.

## Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet. The function relies on global state managed by the parser, such as the current token being processed (`current()`).
- **Return Value**: A `std::unique_ptr<ASTNode>` containing the parsed `ImportStmt`. This allows for safe management of dynamically allocated memory and ensures proper cleanup when the AST node is no longer needed.

## How It Works

1. **Initialization**:
   - The function starts by initializing an `ImportStmt` object named `stmt`.
   - It also retrieves the current line number (`ln`) where the parsing begins.

2. **Parsing Based on Syntax Type**:
   - If the syntax indicates an `import ... from ...` statement (`isFrom` is true):
     - The function expects a module name immediately following the `from` keyword. This is done using `expect(TokenType::IDENTIFIER, "Expected module name after 'from'")`, which will throw an error if the expected token type is not found.
     - After successfully reading the module name, it expects the `import` keyword to follow, ensuring the syntax is correct.
     - The function then enters a loop where it repeatedly reads items to be imported. Each item can optionally have an alias specified after the `as` keyword.
   - If the syntax indicates a direct `import ...` statement (`isFrom` is false):
     - The function sets the `module` field of the `ImportStmt` to an empty string, indicating that the import is happening without specifying a base module.
     - Similar to the previous case, the function enters a loop to read items to be imported, allowing for optional aliases.

3. **Handling Whitespace and Newlines**:
   - After parsing all the import items, the function consumes any trailing whitespace or newlines (`while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON)) consume();`). This ensures that the parser moves past any unnecessary characters before proceeding to the next statement.

4. **Returning the Parsed Statement**:
   - Finally, the function creates an `ASTNode` containing the parsed `ImportStmt` and the original line number (`ln`). It returns a unique pointer to this newly created AST node.

## Edge Cases

- **Missing Tokens**: If the expected tokens (e.g., `TokenType::IDENTIFIER`, `TokenType::IMPORT`, etc.) are missing from the input, the function will call `expect` with an appropriate error message, causing the parser to terminate with an error.
- **Extra Commas**: Extra commas between import items are allowed and ignored, but they must not appear at the end of the list.
- **Whitespace Handling**: The function correctly handles leading and trailing whitespace around tokens, ensuring that the parsed statement is robust against formatting issues.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to extract individual tokens from the source code. This includes identifiers, keywords like `from` and `import`, and punctuation marks like commas and semicolons.
- **Error Handling**: When unexpected tokens are encountered, the function throws errors using the `expect` method, which is part of the parser's error handling mechanism.
- **Abstract Syntax Tree (AST)**: The function constructs an `ImportStmt` object and wraps it in an `ASTNode`. This interaction with the AST is crucial for building the overall structure of the program during compilation.

Overall, the `parseImportStmt` function plays a vital role in interpreting import statements in the Quantum Language, ensuring that the parsed information is correctly represented in the AST for further processing by the compiler.