# TypeChecker Component

The TypeChecker component is a vital part of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This process ensures that the code strictly conforms to the established type rules, thereby eliminating potential runtime type-related errors before execution begins.

## Overview

The main objective of the TypeChecker is to meticulously examine the AST and verify the correctness of variable types, expressions, and function calls. It accomplishes this through a series of interconnected functions:

1. **check**: The overarching function that initiates the type checking process. It sets up the necessary environment and orchestrates the traversal of the AST.
2. **checkNode**: A recursive function that processes individual nodes in the AST. Depending on the node's type, it invokes specific type-checking logic.
3. **checkExpression**: Validates the types of expressions within the AST. This includes arithmetic operations, logical conditions, and function arguments.
4. **checkVariableDeclaration**: Ensures that variable declarations adhere to the specified type constraints.
5. **checkFunctionCall**: Verifies that function calls are made with parameters of the correct types and that the return type matches expectations.

These components work together seamlessly to provide comprehensive type checking coverage across the entire AST.

## File Structure

- `type_checker.h`: Contains the declaration of the `TypeChecker` class and its methods.
- `type_checker.cpp`: Implements the `TypeChecker` class and provides the definitions for all declared methods.
- `expression_type_checker.h`: Declares helper functions specifically tailored for type-checking expressions.
- `expression_type_checker.cpp`: Implements the helper functions for expression type-checking.
- `variable_declaration_type_checker.h`: Declares functions for validating variable declarations.
- `variable_declaration_type_checker.cpp`: Implements the functions for variable declaration type-checking.
- `function_call_type_checker.h`: Declares functions for ensuring proper function call syntax and parameter types.
- `function_call_type_checker.cpp`: Implements the functions for function call type-checking.

## Usage

To use the TypeChecker component, instantiate the `TypeChecker` class and invoke the `check` method with the root of the AST as an argument. The component will then recursively traverse the AST, applying type-checking rules at each node.

```cpp
#include "type_checker.h"

int main() {
    // Create the AST
    Node* ast = createAst();

    // Initialize the TypeChecker
    TypeChecker typeChecker;

    // Perform type checking
    bool isValid = typeChecker.check(ast);

    if (isValid) {
        std::cout << "Type checking passed successfully." << std::endl;
    } else {
        std::cout << "Type checking failed." << std::endl;
    }

    return 0;
}
```

## Contributing

Contributions to the TypeChecker component are welcome! Please follow these guidelines when submitting pull requests:

1. Ensure your changes pass all existing tests.
2. Add new tests if you introduce any new functionality.
3. Maintain consistent coding style throughout the project.

For more detailed information on contributing, please refer to the [CONTRIBUTING](../CONTRIBUTING.md) file.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.