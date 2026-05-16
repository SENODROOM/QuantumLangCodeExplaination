# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is designed to handle the parsing of variable declarations. This function supports both single and multiple variable declarations, allowing for optional type hints and initialization expressions. The primary purpose of this function is to construct an Abstract Syntax Tree (AST) node representing the variable declaration, which can then be used during further compilation stages.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined within the provided code snippet; however, it relies on global state such as the current token being processed and functions like `consume()` and `match()` to determine the next token to process.
  
- **Return Value**:
  - An `ASTNodePtr` pointing to the newly constructed `VarDecl` or `BlockStmt` node representing the parsed variable declaration(s).

## Edge Cases

- **Single Variable Declaration**: When only one variable is declared, the function constructs a single `VarDecl` node.
- **Multiple Variable Declarations**: When multiple variables are declared in a single statement, the function constructs a `BlockStmt` containing multiple `VarDecl` nodes.
- **Missing Type Hint**: If a type hint is not specified, the function leaves the `typeHint` field empty in the `VarDecl` node.
- **Missing Initialization Expression**: If an initialization expression is not specified, the function sets the `init` field to `nullptr` in the `VarDecl` node.
- **Invalid Token Sequence**: If the sequence of tokens does not match the expected pattern for a variable declaration, the function throws a `ParseError`.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve the current token and subsequent tokens in the source code. Tokens include identifiers, keywords, colons, assignment operators, commas, newlines, and semicolons.
- **Expression Parser**: For multi-variable declarations, when an initialization expression is present, the function calls `parseExpr()` to parse the expression and construct an appropriate AST node for it.
- **Error Handling**: The function includes error handling logic to manage unexpected token sequences or invalid syntax during parsing. If an error is detected, a `ParseError` is thrown with details about the expected and encountered tokens.

Overall, the `parseVarDecl` function plays a crucial role in the Quantum Language compiler by accurately parsing variable declarations and constructing the corresponding AST nodes, ensuring that the subsequent compilation steps can proceed correctly.