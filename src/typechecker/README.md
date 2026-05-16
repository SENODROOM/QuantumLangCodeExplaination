# TypeChecker Component

The TypeChecker component is a crucial part of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This ensures that the code complies with the defined type rules, thereby avoiding runtime type-related errors before execution begins.

## Overview

The main objective of the TypeChecker component is to validate the types in the AST nodes and ensure they adhere to the language's type system. It processes the AST by traversing it recursively and applying type rules to each node. If any type error is detected during the traversal, the component reports an error message indicating the issue and halts further processing.

### Files and Their Roles

- **`check.cpp`**: Contains the implementation of the core type checking logic. It defines functions that traverse the AST and apply type rules to each node.
- **`check.h`**: Declares the public interface of the TypeChecker component, including function prototypes for type checking operations.
- **`checkNode.cpp`** and **`checkNode.h`**: These files contain specific implementations for type checking different types of AST nodes such as variables, expressions, statements, etc.

### Overall Flow

1. **Initialization**: The TypeChecker component initializes itself and prepares for type checking.
2. **Traversal**: It starts traversing the AST from the root node downwards.
3. **Type Checking**: For each node, it applies the appropriate type checking rule based on the node's type (e.g., variable declaration, arithmetic expression).
4. **Error Reporting**: If a type error is found, it reports an error message detailing the issue.
5. **Completion**: Once the entire AST has been traversed, the TypeChecker component concludes its operation.

By ensuring that all types in the AST are correctly validated, the TypeChecker component helps maintain the integrity and reliability of the compiled quantum code.

## Usage

To use the TypeChecker component, include the necessary headers and call the `typeCheck` function with the root of your AST.

```cpp
#include "check.h"

// Assuming 'root' is the root node of your AST
bool isValid = typeCheck(root);
if (!isValid) {
    std::cerr << "Type check failed!" << std::endl;
}
```

## Contributing

Contributions to the TypeChecker component are welcome! Please follow the guidelines below:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeatureName`).
3. Make your changes and commit them (`git commit -m "Add some feature"`).
4. Push to the branch (`git push origin feature/YourFeatureName`).
5. Open a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.