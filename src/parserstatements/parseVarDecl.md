# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is responsible for parsing variable declarations. It handles both single and multiple variable declarations, supporting optional type hints and initialization expressions. The function ensures that the syntax of variable declarations adheres to the language's rules and constructs an Abstract Syntax Tree (AST) node representing the declaration.

## Parameters

- **None**: The function does not take any explicit parameters. It relies on the global state maintained by the parser, such as the current token being processed.

## Return Value

- **ASTNodePtr**: The function returns a pointer to an AST node representing the parsed variable declaration. If the declaration includes multiple variables, it returns a pointer to a `BlockStmt` containing all the individual `VarDecl` nodes.

## Edge Cases

- **Single Variable Declaration**: When only one variable is declared, the function parses the variable name and optionally its type hint and initialization expression.
  
- **Multiple Variable Declarations**: The function can handle multiple variable declarations separated by commas. Each variable can have its own type hint and initialization expression.
  
- **Missing Identifier or Type Hint**: If the expected identifier or type hint is missing, the function throws a `ParseError` indicating the error and the line number where it occurred.
  
- **Initialization Without Expression**: If an assignment operator (`=`) is encountered but followed by invalid tokens, the function skips the invalid tokens and continues parsing.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve the next token in the source code. It checks the type of the current token to determine whether it is an identifier or a keyword representing a data type.
  
- **Expression Parser**: For multi-variable declarations, the function calls `parseExpr()` to parse the initialization expression associated with each variable.
  
- **Abstract Syntax Tree (AST)**: The function constructs an `ASTNode` or `BlockStmt` based on the parsed information and adds it to the AST. This allows subsequent stages of compilation to use the structured representation of the variable declarations.

## Implementation Details

1. **Line Number Initialization**: The function starts by recording the current line number using `current().line`.

2. **Variable Name Parsing**:
   - If the current token is an identifier, it consumes the token and assigns its value to `name`.
   - If the current token is a C-type keyword (e.g., `int`, `float`), it also consumes the token and assigns its value to `name`.
   - If neither condition is met, it throws a `ParseError` indicating that a variable name was expected.

3. **Optional Type Hint Parsing**:
   - If the next token is a colon (`:`), it consumes the colon and then expects either an identifier or a C-type keyword to represent the type hint.
   - If these conditions are met, it consumes the token and assigns its value to `typeHint`. If not, it leaves `typeHint` empty.

4. **Optional Initialization Expression Parsing**:
   - If the next token is an assignment operator (`=`), it calls `parseExpr()` to parse the initialization expression and stores the result in `init`.

5. **Multi-Variable Handling**:
   - If the next token is a comma (`,`), it enters a loop to handle multiple variable declarations.
   - Inside the loop, it creates a new `BlockStmt` and appends a `VarDecl` node for each variable to the block's statements.
   - It continues consuming commas and parsing variables until it encounters a newline (`\n`) or semicolon (`;`).

6. **Whitespace Skipping**:
   - The function calls `skipNewlines()` to skip over any whitespace characters before checking for the end of the statement.

7. **Error Handling**:
   - If the loop encounters unexpected tokens or reaches the end of the input, it breaks out of the loop and continues parsing the rest of the statement.

By handling both single and multiple variable declarations, providing support for type hints and initialization expressions, and ensuring robust error handling, the `parseVarDecl` function plays a crucial role in accurately parsing variable declarations in the Quantum Language compiler.