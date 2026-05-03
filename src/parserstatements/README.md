# ParserStatements Component of Quantum Language Compiler

The `ParserStatements` component is a vital part of the Quantum Language compiler responsible for parsing and interpreting various statement types within the quantum programming language. This component covers control structures such as loops and conditionals, expressions, variable declarations, and more.

## Overview

The `ParserStatements` component processes different types of statements encountered during the compilation of a quantum program. It ensures that each statement is correctly parsed according to the syntax rules of the quantum language. The component interacts with other parts of the compiler, including the lexer and semantic analyzer, to build an abstract syntax tree (AST) that represents the structure of the quantum code.

### Key Components

- **parseBodyOrStatement**: Parses either a block of statements or a single statement.
- **parseVarDecl**: Parses variable declarations.
- **parseIfStmt**: Parses conditional statements (`if`, `else`).
- **parseWhileStmt**: Parses loop statements (`while`).
- **parseReturnStmt**: Parses return statements.
- **parsePrintStmt**: Parses print statements.
- **parseInputStmt**: Parses input statements.
- **parseCoutStmt**: Parses output statements using `std::cout`.
- **parseCinStmt**: Parses input statements using `std::cin`.
- **parseImportStmt**: Parses import statements to include external libraries.
- **parseExprStmt**: Parses expression statements.
- **parseExpr**: Parses individual expressions.

### File Structure

The component is organized into several files, each handling specific types of statements:

- `body_or_statement.cpp`: Contains the implementation for parsing blocks of statements or single statements.
- `variable_declaration.cpp`: Handles the parsing of variable declarations.
- `conditional_statement.cpp`: Implements parsing for conditional statements (`if`, `else`).
- `loop_statement.cpp`: Manages parsing of loop statements (`while`).
- `return_statement.cpp`: Processes return statements.
- `print_statement.cpp`: Parses print statements.
- `input_statement.cpp`: Handles input statements.
- `output_statement.cpp`: Manages output statements using `std::cout` and `std::cin`.
- `import_statement.cpp`: Processes import statements to include external libraries.
- `expression_statement.cpp`: Parses expression statements.
- `expression.cpp`: Manages the parsing of individual expressions.

Each file contains detailed documentation on its functionality, parameters, and return values. This ensures that developers can understand and use the component effectively.

## Usage

To use the `ParserStatements` component, follow these steps:

1. **Include the Header Files**:
   ```cpp
   #include "parserstatements/body_or_statement.h"
   #include "parserstatements/variable_declaration.h"
   // Include other necessary header files
   ```

2. **Initialize the Parser**:
   ```cpp
   Parser parser;
   ```

3. **Parse Statements**:
   ```cpp
   StatementPtr stmt = parser.parseBodyOrStatement();
   // Use the parsed statement
   ```

4. **Handle Errors**:
   The parser throws exceptions when it encounters errors. Catch these exceptions to handle them appropriately.
   ```cpp
   try {
       StatementPtr stmt = parser.parseBodyOrStatement();
   } catch (const std::exception& e) {
       std::cerr << "Error parsing statement: " << e.what() << std::endl;
   }
   ```

## Example

Here's a simple example demonstrating how to use the `ParserStatements` component:

```cpp
#include <iostream>
#include "parserstatements/body_or_statement.h"

int main() {
    try {
        Parser parser;
        StatementPtr stmt = parser.parseBodyOrStatement();
        // Process the parsed statement
        std::cout << "Parsed statement successfully." << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error parsing statement: " << e.what() << std::endl;
    }

    return 0;
}
```

## Contributing

Contributions to the `ParserStatements` component are welcome! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request.

## License

This component is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Feel free to update the existing README with the above content.