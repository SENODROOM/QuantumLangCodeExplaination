# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is responsible for parsing variable declarations. It handles both single and multi-variable declarations, including those with type hints and initializers.

## Parameters/Return Value

- **Parameters**:
  - None explicitly declared within the function signature; it operates on global state managed by the parser.
  
- **Return Value**:
  - An `ASTNodePtr` pointing to an `ASTNode` representing the parsed variable declaration(s). If multiple variables are declared, it returns a `BlockStmt` containing all the variable declarations.

## Why It Works This Way

### Single Variable Declaration

1. **Variable Name Parsing**: The function first checks if the current token is an identifier or a C-type keyword. If either is found, it consumes the token and assigns its value to the `name` variable. If not, it throws a `ParseError`.

2. **Type Hint Parsing**: Next, it looks for a colon (`:`) indicating a type hint. If found, it consumes the colon and then parses another identifier or C-type keyword as the type hint, assigning it to the `typeHint` variable.

3. **Initializer Parsing**: The function then checks for an assignment operator (`=`). If found, it calls `parseExpr()` to parse the initializer expression and stores the result in `init`.

4. **Single Variable Node Creation**: Finally, it creates an `ASTNode` of type `VarDecl` using the parsed `name`, `init`, and `typeHint`. If there was no type hint provided, `typeHint` remains empty.

### Multi-Variable Declaration

1. **Block Initialization**: For multi-variable declarations, the function initializes a unique pointer to a `BlockStmt` object. This allows for grouping multiple variable declarations together in a single AST node.

2. **First Variable Addition**: The first variable declaration is added to the block's statements list as described above.

3. **Loop for Additional Variables**: After adding the first variable, the function enters a loop that continues until it encounters a newline character (`NEWLINE`), semicolon (`SEMICOLON`), or reaches the end of the input.

   - Inside the loop, it skips any newlines using `skipNewlines()`.
   
   - It checks for the end of the input or a newline/semicolon to break out of the loop.
   
   - It then parses subsequent variable names and optional type hints and initializers in a manner similar to the single variable case.
   
   - Each parsed variable declaration is added to the block's statements list.

4. **Post-Loop Consumption**: After exiting the loop, the function consumes any remaining newline characters or semicolons to ensure proper parsing of the surrounding context.

## Edge Cases

- **Missing Variable Name**: If the current token is neither an identifier nor a C-type keyword when expected, a `ParseError` is thrown.
  
- **Invalid Type Hint**: If a colon is present but followed by something other than an identifier or a C-type keyword, the declaration is considered invalid, and the loop breaks.
  
- **No Initializer**: If no assignment operator is found after the variable name, the initializer will be `nullptr`.

- **Trailing Commas**: Trailing commas in the declaration list are ignored.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. It uses functions like `current()`, `consume()`, and `check()` to interact with the tokenizer's state.
  
- **Expression Parser**: When an initializer is present, the function calls `parseExpr()` to parse the expression following the assignment operator. This requires interaction with the expression parser component.
  
- **AST Construction**: The parsed variable declarations are constructed into an Abstract Syntax Tree (AST) using `ASTNode` objects. The `BlockStmt` container is used for multi-variable declarations, ensuring they are treated as a group in the AST.

This function is crucial for handling variable declarations in the Quantum Language, providing flexibility for both simple and complex declarations while maintaining robust error handling and interaction with other parts of the compiler.