# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical component of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This phase ensures that all expressions and statements adhere to the defined types, thereby preventing runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The TypeChecker operates during the semantic analysis stage of the compilation process. It traverses the AST, examining each node's type and ensuring it conforms to the language's type rules. The results of type checking are used to generate more accurate machine code and to provide early feedback to developers about potential issues in their code.

## Key Design Decisions

### Use of Shared Pointers for Environment Management

**WHY:** Utilizing `std::shared_ptr` for managing the type environment provides several advantages. Firstly, it simplifies memory management by automatically handling the deallocation of memory when shared pointers go out of scope. Secondly, it facilitates efficient sharing of environments between different parts of the compiler, reducing redundancy and improving performance. Lastly, it supports concurrent access to the environment without the need for explicit locking mechanisms, enhancing scalability.

### Simplified Function Type Representation

**WHY:** In the current implementation, functions are represented as simply "fn" in the type environment. This decision was made to streamline the type system and reduce complexity. However, it limits the ability to distinguish between different function signatures at compile time. Future enhancements may involve more detailed representation of function types to support advanced features like polymorphism and higher-order functions.

## Documentation of Major Classes/Functions

### Class: TypeChecker

**Purpose:** The primary class responsible for performing static type checking on the AST.

**Behaviour:** 
- Initializes the type environment with built-in functions.
- Traverses the AST and checks each node's type against the defined rules.
- Handles variable declarations and function definitions, ensuring type consistency.
- Provides warnings for type mismatches and updates the type environment accordingly.

### Function: check(const std::vector<ASTNodePtr>& nodes)

**Purpose:** Checks an entire list of AST nodes for type correctness.

**Behaviour:** Iterates through each node in the provided vector and calls `checkNode` to perform the actual type checking.

### Function: check(const ASTNodePtr& node)

**Purpose:** Checks a single AST node for type correctness.

**Behaviour:** If the node is a block statement, it recursively checks each statement within the block. Otherwise, it directly calls `checkNode`.

### Function: checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)

**Purpose:** Recursively checks an individual AST node for type correctness using the specified type environment.

**Behaviour:** Based on the type of the node, it performs specific checks:
- For literals (numbers, strings, booleans), it returns their respective types.
- For identifiers, it resolves their type from the environment.
- For variable declarations, it checks the initializer's type against the declared type hint and updates the environment.
- For function declarations, it sets up a new environment for the function parameters and checks the body.
- For binary expressions, it checks the types of both operands and ensures they are compatible.

## Tradeoffs or Limitations

- **Simplified Function Types:** Representing functions as "fn" rather than detailed signatures simplifies the type system but limits the expressiveness of type checking.
- **Memory Overhead:** Using `std::shared_ptr` for environment management introduces some overhead compared to raw pointers, which might not be necessary for very small programs.
- **Concurrency:** While `std::shared_ptr` supports concurrent access, it may not be optimal for highly parallel scenarios where frequent modifications to the environment are required.

## Usage Example

To use the `TypeChecker`, you would typically instantiate it and call its `check` method with the root node of your AST:

```cpp
#include "TypeChecker.h"
#include "Parser.h"

int main() {
    Parser parser;
    ASTNodePtr ast = parser.parse("your_source_code.q");

    TypeChecker checker;
    checker.check(ast);

    return 0;
}
```

This example assumes the existence of a `Parser` class capable of parsing source code into an AST.

## Conclusion

The `TypeChecker.cpp` file plays a vital role in ensuring the type safety of the Quantum Language compiler. By leveraging `std::shared_ptr` for environment management and providing basic type checking, it helps catch errors early and contributes to the overall reliability of the compiled code. Although there are some limitations and areas for improvement, the current design serves well for the intended purposes and forms the foundation for more sophisticated type systems in future iterations.