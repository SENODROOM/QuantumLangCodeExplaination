# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is designed to handle the parsing of variable declarations. This function supports both single and multiple variable declarations, allowing for optional type hints and initializations.

### Parameters

- **None**: The function takes no explicit parameters.

### Return Value

- **ASTNodePtr**: A pointer to an abstract syntax tree node representing the parsed variable declaration(s).

## Why It Works This Way

The function works by first checking if the current token is either an identifier or a C-type keyword, which would indicate the start of a variable name. If not, it throws a `ParseError`. After successfully identifying the variable name, it looks for an optional colon followed by another identifier or C-type keyword to determine if a type hint is provided. If an assignment operator (`=`) is found, it parses the expression that follows as the initialization of the variable. For multi-variable declarations, it continues to parse subsequent variables until a comma is no longer present, and then consumes any trailing newlines or semicolons.

### Edge Cases

- **Single Variable Declaration**: Handles cases where only one variable is declared.
- **Multiple Variable Declarations**: Supports scenarios where multiple variables are declared on a single line separated by commas.
- **Missing Type Hint**: Allows for variable declarations without specifying a type hint.
- **Missing Initialization**: Permits variable declarations without providing an initial value.
- **Syntax Errors**: Throws errors if the expected tokens are missing or incorrect.

### Interactions With Other Components

- **Tokenizer**: Uses the tokenizer to get the current token and advance through the input stream.
- **Abstract Syntax Tree (AST)**: Constructs an ASTNode representing the variable declaration(s).
- **Error Handling**: Utilizes the `ParseError` class to report syntax errors encountered during parsing.

This function ensures that the parser can correctly interpret variable declarations in various forms, making it robust for different types of input.