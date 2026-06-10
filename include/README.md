# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential component of the QuantumLanguage compiler, focusing on the Type Checking phase. This phase validates the types of variables, expressions, and statements within the source code to ensure they conform to the language's type system rules. The Type Checker helps catch errors early in the compilation process, improving code quality and reducing debugging time.

## Role in Compiler Pipeline

The Type Checker operates during the semantic analysis stage of the compiler pipeline, following the Lexical Analysis and Syntax Parsing phases. Its primary responsibilities include:

1. **Static Type Checking**: Ensures all variables, functions, and data structures have correct types before any code generation occurs.
2. **Error Detection**: Identifies type mismatches, undeclared variables, and other issues that could lead to runtime errors.
3. **Type Inference**: Where possible, infers the types of expressions based on their usage and context.

By performing these tasks, the Type Checker contributes significantly to the robustness and reliability of the generated quantum code.

## Key Design Decisions and Why

### 1. Use of Exception Handling

The Type Checker uses custom exception classes (`StaticTypeError`) derived from `std::runtime_error`. This approach allows for clear error messages that include the line number where the error occurred, making it easier for developers to locate and fix issues.

```cpp
class StaticTypeError : public std::runtime_error
{
public:
    int line;
    StaticTypeError(const std::string &msg, int l)
        : std::runtime_error(msg), line(l) {}
};
```

**Why**: Custom exceptions provide more specific information about errors, which aids in debugging and improves the overall user experience.

### 2. Hierarchical Type Environment

A hierarchical type environment (`TypeEnv`) is implemented using a shared pointer to a parent environment. This structure supports nested scopes, allowing variables defined in inner scopes to shadow those in outer scopes.

```cpp
struct TypeEnv {
    std::map<std::string, std::string> vars;
    std::shared_ptr<TypeEnv> parent;

    TypeEnv(std::shared_ptr<TypeEnv> p = nullptr) : parent(p) {}

    void define(const std::string& name, const std::string& type) {
        vars[name] = type;
    }

    std::string resolve(const std::string& name) {
        if (vars.count(name)) return vars[name];
        if (parent) return parent->resolve(name);
        return "any";
    }
};
```

**Why**: Hierarchical scoping is necessary for languages with block structures, ensuring that variable resolution follows the standard scope rules.

### 3. Modular Design

The Type Checker is designed as a modular class (`TypeChecker`). It includes methods to check entire ASTs (`check(const std::vector<ASTNodePtr>& nodes)`) and individual AST nodes (`check(const ASTNodePtr& node)`).

```cpp
class TypeChecker
{
public:
    TypeChecker();
    void check(const std::vector<ASTNodePtr>& nodes);
    void check(const ASTNodePtr& node);
    std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env);

private:
    std::shared_ptr<TypeEnv> globalEnv;
};
```

**Why**: A modular design makes the Type Checker easier to maintain and extend, allowing for different parts of the compiler to interact with it independently.

## Major Classes/Functions Overview

### `TypeChecker`

- **Constructor**: Initializes the global type environment.
- **Methods**:
  - `void check(const std::vector<ASTNodePtr>& nodes)`: Checks the entire list of AST nodes.
  - `void check(const ASTNodePtr& node)`: Checks a single AST node.
  - `std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks a node within a given type environment.

### `TypeEnv`

- **Attributes**:
  - `std::map<std::string, std::string> vars`: Stores variable names and their types.
  - `std::shared_ptr<TypeEnv> parent`: Points to the parent environment, enabling nested scopes.
  
- **Methods**:
  - `void define(const std::string& name, const std::string& type)`: Defines a new variable in the current environment.
  - `std::string resolve(const std::string& name)`: Resolves the type of a variable, considering nested environments.

## Tradeoffs

### Memory Usage

Using a hierarchical type environment can increase memory usage due to the overhead of storing multiple environment instances. However, this tradeoff is justified by the need to support nested scopes and avoid potential conflicts between variable names.

### Complexity

Implementing a custom exception class adds complexity to the codebase but enhances its readability and maintainability. Similarly, the modular design increases complexity but provides better separation of concerns and scalability.

Overall, the benefits of clear error messages, proper scope management, and modularity outweigh the minor drawbacks of increased memory usage and complexity.