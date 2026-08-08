# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is designed to parse variable declarations. This function supports both single and multiple variable declarations, allowing for optional type hints and initialization expressions. The primary goal of this function is to accurately interpret the syntax of variable declarations as specified by the Quantum Language grammar rules.

## Parameters

- **None**: The `parseVarDecl` function does not take any explicit parameters. Instead, it relies on the global state maintained by the parser, such as the current token being processed, to determine the structure of the variable declaration.

## Return Value

- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an Abstract Syntax Tree (AST) node representing the parsed variable declaration. If the declaration involves multiple variables, it returns a block statement containing all the individual variable declarations.

## Edge Cases

- **Single Variable Declaration**: When only one variable is declared, the function will parse the variable name, optional type hint, and initialization expression (if present). For example:
  ```quantum
  let x = 5;
  ```
  In this case, the function will create a single `VarDecl` node.

- **Multiple Variable Declarations**: When multiple variables are declared within a single statement, they must be separated by commas. Each variable can have its own type hint and initialization expression. For example:
  ```quantum
  const W = 60, H = 24;
  ```
  Here, the function will create two `VarDecl` nodes within a `BlockStmt`.

- **Missing Initialization or Type Hint**: The function allows for optional initialization and type hints. If these are missing, the corresponding fields in the `VarDecl` node will be empty. For example:
  ```quantum
  let x;
  ```
  In this case, the `init` field of the `VarDecl` node will be `nullptr`.

- **Syntax Errors**: If the syntax of the variable declaration is incorrect, the function will throw a `ParseError`. For example, if the variable name is missing after `let`, the function will raise an error indicating that a variable name was expected.

## Interactions with Other Components

- **Tokenizer**: The `parseVarDecl` function interacts with the tokenizer to obtain the next token in the input stream. It uses functions like `current()` and `consume()` to manage the tokens.

- **Grammar Rules**: The function adheres to the Quantum Language grammar rules for variable declarations. These rules dictate the structure of valid variable declarations, including the use of keywords like `const` and `let`, optional type hints, and initialization expressions.

- **Error Handling**: The function includes robust error handling mechanisms to detect and report syntax errors. If an unexpected token is encountered during parsing, it throws a `ParseError` with appropriate details about the error location and message.

In summary, the `parseVarDecl` function is crucial for correctly interpreting variable declarations in the Quantum Language. By supporting both single and multiple declarations, optional type hints, and initialization expressions, it ensures that the compiler can handle a wide range of valid syntax structures. Its interaction with the tokenizer and adherence to the language's grammar rules make it a vital component of the parser module.