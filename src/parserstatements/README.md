# ParserStatements Component of Quantum Language Compiler

The `ParserStatements` component is a critical module within the Quantum Language compiler, responsible for parsing and interpreting various statement types in the quantum programming language. This component handles fundamental control structures such as loops, conditionals, variable declarations, expressions, and more, ensuring that the code adheres to the language's syntax and semantics.

## Overview

- **parseBodyOrStatement**: Parses either a block body or a single statement.
- **parseVarDecl**: Parses variable declarations.
- **parseIfStmt**: Parses conditional statements (`if`, `else`, `elif`).
- **parseWhileStmt**: Parses loop statements (`while`).
- **parseReturnStmt**: Parses return statements.
- **parsePrintStmt**: Parses print statements.
- **parseInputStmt**: Parses input statements.
- **parseCoutStmt**: Parses output statements using `cout`.
- **parseCinStmt**: Parses input statements using `cin`.
- **parseImportStmt**: Parses import statements.
- **parseExprStmt**: Parses expression statements.
- **parseExpr**: Parses expressions within statements.

## Directory Structure

```
parserstatements/
├── include/
│   ├── parserstatements.h
│   └── ...
├── src/
│   ├── parserstatements.cpp
│   └── ...
└── tests/
    ├── test_parserstatements.cpp
    └── ...
```

## How It Works

1. **Initialization**: The component initializes itself with necessary data structures and configurations required for parsing.
2. **Parsing Statements**:
   - **parseBodyOrStatement**: Determines whether to parse a block body or a single statement based on the context.
   - **parseVarDecl**: Processes variable declarations, extracting their names and types.
   - **parseIfStmt**: Handles conditional logic, including multiple conditions (`elif`) and an optional else block.
   - **parseWhileStmt**: Manages loop constructs, processing the condition and the loop body.
   - **parseReturnStmt**: Extracts the value to be returned from a function.
   - **parsePrintStmt**: Parses print statements to determine the values to be printed.
   - **parseInputStmt**: Handles user input, determining the variables to store the input values.
   - **parseCoutStmt**: Processes output statements using `cout`, formatting the output as specified.
   - **parseCinStmt**: Parses input statements using `cin`, handling the extraction of input values into variables.
   - **parseImportStmt**: Manages the importation of modules or libraries.
   - **parseExprStmt**: Parses statements containing expressions, evaluating them if necessary.
   - **parseExpr**: Recursively parses expressions, handling operators, operands, and function calls.
3. **Error Handling**: The component includes mechanisms to handle errors during parsing, providing meaningful error messages and stopping the compilation process if necessary.
4. **Output**: After successful parsing, the component generates intermediate representation (IR) or directly translates the parsed statements into executable code.

## Dependencies

- **Lexer**: Provides tokens for parsing.
- **SymbolTable**: Manages symbols and their scopes.
- **ErrorManager**: Handles reporting and managing errors during parsing.

## Usage

To use the `ParserStatements` component, you typically integrate it into the main compiler pipeline after tokenization. Here’s a simplified example:

```cpp
#include "parserstatements.h"

int main() {
    // Initialize lexer and symbol table
    Lexer lexer("input.q");
    SymbolTable symbolTable;

    // Create parser instance
    ParserStatements parser(lexer, symbolTable);

    // Parse the entire file
    parser.parseFile();

    // Output IR or generate executable code
    // ...

    return 0;
}
```

## Testing

Unit tests for the `ParserStatements` component are located in the `tests/` directory. These tests ensure that individual functions correctly parse and interpret different types of statements. To run the tests, execute:

```sh
make test
```

This will compile and run all the unit tests, verifying the functionality of the parser statements.

## Contribution

Contributions to the `ParserStatements` component are welcome! If you find bugs, have suggestions for improvements, or want to add new features, please follow these steps:

1. Fork the repository.
2. Create a new branch for your changes.
3. Make your modifications.
4. Write unit tests for your new functionality.
5. Submit a pull request.

By following these guidelines, you can help enhance the robustness and capabilities of the Quantum Language compiler.