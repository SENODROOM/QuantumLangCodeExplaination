# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is responsible for parsing variable declarations. It handles both single and multiple variable declarations, supporting optional type hints and initializers. The function ensures that the syntax of variable declarations adheres to the language rules, throwing appropriate errors if the syntax is incorrect.

## Parameters

- **None**: The function does not take any explicit parameters. Instead, it relies on the global state managed by the parser, such as the current token being processed.

## Return Value

- **ASTNodePtr**: The function returns an `ASTNodePtr`, which is a smart pointer to an abstract syntax tree node representing the parsed variable declaration. If multiple variables are declared, it returns a `BlockStmt` containing all the individual `VarDecl` nodes.

## Edge Cases

1. **Single Variable Declaration**:
   - The function can handle a single variable declaration without any issues. For example, `let x = 5`.
   
2. **Multiple Variable Declarations**:
   - The function supports multiple variable declarations separated by commas. For example, `const W = 60, H = 24`.
   - However, if there is a missing comma or an unexpected token between declarations, the function will stop parsing at that point and return the parsed declarations up to that error.

3. **Optional Type Hint**:
   - A variable declaration can optionally include a type hint using a colon followed by a type identifier. For example, `let x: int = 5`.
   - If the type hint is omitted, the function will proceed without it.

4. **Missing Initializer**:
   - A variable declaration can be made without an initializer. For example, `let x;`
   - In this case, the initializer part of the `VarDecl` node will be set to `nullptr`.

5. **Unexpected Tokens**:
   - If the parser encounters tokens that do not conform to the expected syntax for variable declarations, such as a semicolon where a comma should be, it will throw a `ParseError` indicating the expected token and the actual token encountered.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to get the current token and advance through the tokens as it parses the variable declarations.
- **Error Handling**: The function interacts with the error handling mechanism of the parser to report syntax errors when the input does not match the expected variable declaration pattern.
- **Abstract Syntax Tree (AST)**: The function constructs an AST based on the parsed variable declarations. Each variable declaration results in an `ASTNode` of type `VarDecl`. If multiple declarations are found, they are grouped into a `BlockStmt`.

## Implementation Details

Here’s a breakdown of how the `parseVarDecl` function works:

1. **Initialization**:
   - The function starts by recording the line number (`ln`) of the current token.
   - It attempts to consume an identifier or a C-type keyword to identify the variable name.

2. **Type Hint Parsing**:
   - If a colon (`:`) is encountered, the function consumes it and then expects another identifier or C-type keyword to parse the type hint.

3. **Initializer Parsing**:
   - If an assignment operator (`=`) is found, the function calls `parseExpr()` to parse the expression following the assignment and assigns it to the `init` member of the `VarDecl` node.

4. **Multi-variable Parsing**:
   - If a comma (``,``) is encountered after the first variable declaration, the function enters a loop to parse additional variables.
   - Inside the loop, it continues to consume identifiers or C-type keywords for each subsequent variable name.
   - It also checks for optional type hints and initializers for these variables.
   - Each parsed variable is added to a `BlockStmt` node, which is returned at the end of the function.

5. **Whitespace Handling**:
   - The function skips newlines and semicolons after parsing multiple variable declarations to ensure proper separation and avoid premature termination.

By following this structured approach, the `parseVarDecl` function effectively parses variable declarations according to the Quantum Language syntax rules, ensuring that the resulting AST accurately represents the parsed code.