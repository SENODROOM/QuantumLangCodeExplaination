# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is designed to parse variable declarations. This function supports both single and multi-variable declarations, allowing for optional type hints and initializers. The primary purpose of this function is to correctly interpret and construct an Abstract Syntax Tree (AST) node representing a variable declaration based on the input tokens provided by the lexer.

## Parameters

- **None**: The function does not take any explicit parameters. Instead, it relies on the global state maintained by the parser, such as the current token being processed, to determine the structure of the variable declaration.

## Return Value

- **ASTNodePtr**: A pointer to an ASTNode that represents the parsed variable declaration. If the declaration includes multiple variables, it returns a BlockStmt containing all the individual VarDecl nodes.

## Edge Cases

1. **Single Variable Declaration**:
   - The function can handle a simple variable declaration without a type hint or initializer, e.g., `let x`.
   
2. **Multi-Variable Declaration**:
   - The function supports declaring multiple variables within a single statement, separated by commas, e.g., `const W = 60, H = 24`.

3. **Optional Type Hint**:
   - The function allows for an optional type hint after the variable name, e.g., `let x: int`, but it also accepts declarations without type hints.

4. **Initializer**:
   - The function can parse and include an initializer for the variable, e.g., `let x = 1`.

5. **Syntax Errors**:
   - If the expected syntax is not found (e.g., expecting a variable name but encountering a different token), the function throws a `ParseError` detailing the issue.

## Interactions with Other Components

- **Lexer**: The function consumes tokens from the lexer to identify the variable name, type hint, and potential initializer. The lexer provides the sequence of tokens that the parser processes to build the AST.

- **Token Types**: The function uses various token types such as `TokenType::IDENTIFIER`, `TokenType::COLON`, and `TokenType::ASSIGN`. These token types help in distinguishing between different parts of the variable declaration (variable name, type hint, and initializer).

- **Abstract Syntax Tree (AST)**: The function constructs an AST by creating `VarDecl` nodes for each variable declared. For multi-variable declarations, it creates a `BlockStmt` containing all the `VarDecl` nodes. This AST is then used by subsequent phases of the compiler to generate machine code or perform further optimizations.

- **Error Handling**: The function incorporates error handling mechanisms to manage unexpected tokens or malformed declarations. When an error is detected, it throws a `ParseError` which can be caught and handled by higher-level components of the compiler.

In summary, the `parseVarDecl` function plays a crucial role in the Quantum Language compiler by accurately parsing variable declarations and constructing corresponding AST nodes. Its design ensures flexibility and robustness against various syntax patterns and errors, facilitating efficient compilation of Quantum Language programs.