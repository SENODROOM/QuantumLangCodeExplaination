# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential part of the QuantumLanguage compiler, focusing on type checking during the compilation process. This file plays a pivotal role in ensuring that the code adheres to the defined type system, thereby preventing runtime errors due to type mismatches. It provides the necessary infrastructure for the compiler to validate the types of variables, expressions, and function calls, enhancing the reliability and robustness of the generated quantum programs.

## Key Design Decisions

### 1. Use of Exceptions for Error Handling

**WHY:** Using exceptions for error handling allows the `TypeChecker` to throw a `StaticTypeError` when it encounters a type mismatch or other static typing issues. This approach ensures that any type-related errors are caught early in the compilation process, making debugging easier and more efficient.

### 2. Hierarchical Type Environment (`TypeEnv`)

**WHY:** A hierarchical type environment is implemented using a shared pointer to a parent environment. This design choice facilitates the management of variable scopes and nested environments, allowing the compiler to correctly resolve variable types based on their scope. For example, local variables can shadow global variables, and nested functions can access outer function variables.

### 3. Lazy Type Resolution

**WHY:** The `TypeChecker` uses lazy type resolution, where the actual type checking is deferred until the type of a node is explicitly needed. This strategy helps in optimizing performance by avoiding unnecessary checks on nodes that may not be executed at runtime.

## Classes and Functions Documentation

### Class: `StaticTypeError`

**Purpose:** Represents a static type error encountered during the compilation process. It inherits from `std::runtime_error` and adds a line number attribute to provide more context about the error location.

**Behavior:** Constructs a new `StaticTypeError` object with a message and a line number.

```cpp
class StaticTypeError : public std::runtime_error
{
public:
    int line;
    StaticTypeError(const std::string &msg, int l)
        : std::runtime_error(msg), line(l) {}
};
```

### Struct: `TypeEnv`

**Purpose:** Manages the type environment for variables, including their names and types. It supports hierarchical scoping through a parent environment.

**Behavior:**
- Defines a new variable with its type.
- Resolves the type of a variable by searching in the current environment and then recursively in the parent environment. If the variable is not found, it defaults to `"any"`.

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

### Class: `TypeChecker`

**Purpose:** Responsible for performing type checking on the Abstract Syntax Tree (AST) nodes of the QuantumLanguage program.

**Behavior:**
- Initializes the type checker with a global type environment.
- Checks the entire AST by iterating over all nodes.
- Checks individual AST nodes and resolves their types using the provided type environment.

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

## Tradeoffs and Limitations

- **Performance Overhead:** The use of exceptions for error handling introduces some overhead compared to traditional error codes. However, this is mitigated by the fact that most type errors are caught early in the compilation process.
- **Complexity:** Implementing a hierarchical type environment increases the complexity of the type checker. While this enhances the accuracy of type resolution, it also requires careful management of memory and scope resolution.
- **Lazy Evaluation:** Lazy type resolution optimizes performance but might introduce subtle bugs if not handled properly. The compiler must ensure that types are resolved before they are used, otherwise, runtime errors could occur.

## Conclusion

The `include/TypeChecker.h` header file is a critical component of the QuantumLanguage compiler, providing robust type checking mechanisms to ensure the correctness of the generated quantum programs. By leveraging exceptions for error handling, a hierarchical type environment, and lazy evaluation, the type checker offers a reliable and efficient solution to static typing in the compiler pipeline.