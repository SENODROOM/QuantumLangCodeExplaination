# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential part of the QuantumLanguage compiler, dedicated to performing static type checking on abstract syntax tree (AST) nodes. This process ensures that the code adheres to the defined type constraints before it is executed, thereby preventing runtime errors related to type mismatches.

## Role in Compiler Pipeline

### Key Steps

1. **Parsing**: The source code is parsed into an AST.
2. **Semantic Analysis**: The `TypeChecker` class performs semantic analysis on the AST to ensure type correctness.
3. **Code Generation**: If the type checking passes, the next step is code generation.

### Why Semantic Analysis?

Semantic analysis is critical because it checks not just the syntactic structure of the code but also its meaning and adherence to language rules. By identifying type errors early during the compilation phase, the compiler can save time and resources that would otherwise be spent debugging runtime issues.

## Major Classes/Functions Overview

### 1. `StaticTypeError`
- **Role**: Inherits from `std::runtime_error` and is used to throw exceptions when a static type error is detected.
- **Attributes**:
  - `int line`: Stores the line number where the error occurred.
- **Constructor**:
  - `StaticTypeError(const std::string &msg, int l)`: Initializes the exception with a message and the line number.

### 2. `TypeEnv`
- **Role**: Represents the type environment, which tracks variable types within different scopes.
- **Attributes**:
  - `std::map<std::string, std::string> vars`: Maps variable names to their types.
  - `std::shared_ptr<TypeEnv> parent`: Points to the parent scope's type environment.
- **Methods**:
  - `void define(const std::string& name, const std::string& type)`: Defines a new variable in the current scope.
  - `std::string resolve(const std::string& name)`: Resolves the type of a variable by looking up its definition in the current or parent scope. Returns "any" if the variable is not found.

### 3. `TypeChecker`
- **Role**: Manages the overall type checking process, including setting up the global type environment and invoking type checking on individual nodes.
- **Attributes**:
  - `std::shared_ptr<TypeEnv> globalEnv`: Holds the global type environment.
- **Methods**:
  - `TypeChecker()`: Constructor initializes the global type environment.
  - `void check(const std::vector<ASTNodePtr>& nodes)`: Checks a list of AST nodes.
  - `void check(const ASTNodePtr& node)`: Checks a single AST node.
  - `std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks an AST node and returns its type.

## Tradeoffs

### Advantages

- **Preventing Runtime Errors**: Early detection of type errors through static type checking reduces the likelihood of runtime crashes.
- **Improved Code Quality**: Ensures that the codebase is more robust and easier to maintain due to explicit type declarations.
- **Enhanced Developer Experience**: Provides clear, actionable error messages directly at the point of declaration, aiding in quicker bug resolution.

### Disadvantages

- **Increased Complexity**: Implementing a comprehensive type checker requires handling multiple type systems and complex interactions between them.
- **Performance Overhead**: While generally minimal, type checking does add some overhead to the compilation process, especially for large codebases.
- **Maintenance Cost**: As the language evolves, maintaining and updating the type checker becomes increasingly challenging.

Overall, the `TypeChecker.h` file plays a vital role in ensuring the reliability and quality of the QuantumLanguage compiler's output by enforcing strict type constraints during the semantic analysis stage.