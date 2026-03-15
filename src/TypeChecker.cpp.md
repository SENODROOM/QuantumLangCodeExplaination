# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a crucial part of the Quantum Language compiler, designed to perform static type checking on the Abstract Syntax Tree (AST). This phase ensures that all expressions and statements conform to the specified types, thus mitigating potential runtime errors caused by type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis stage of the compilation process. Its main responsibility is to traverse the AST and verify that each expression and statement adheres to the correct data types as defined in the program. If any inconsistencies are found, it reports them as warnings or errors, ensuring the integrity and correctness of the compiled code.

## Key Design Decisions

### Use of Environment for Scoping

**WHY:** To manage variable and function scopes effectively, a nested environment system was implemented. Each scope has its own environment where variables and functions can be defined and resolved. This allows for proper handling of shadowing and scoping rules, which are essential for maintaining the semantics of the language.

### Static vs. Dynamic Typing

**WHY:** The choice between static and dynamic typing was influenced by the need for robust compile-time error detection versus the flexibility of runtime type resolution. Given the complexity and performance considerations of quantum computing, static typing was deemed more appropriate to catch type-related issues early in the development cycle.

### Built-in Functions Handling

**WHY:** Built-in functions such as `print`, `input`, `len`, `sha256`, and `aes128` require special handling because they operate outside the normal scope of user-defined functions. By defining these functions in the global environment at initialization, the `TypeChecker` can ensure that their usage is consistent with the expected types, enhancing the reliability of the compiler's output.

## Documentation of Major Classes/Functions

### Class: TypeChecker

**Purpose:** Manages the overall type-checking process, including setting up the global environment and traversing the AST.

**Behavior:** Initializes with a global environment containing built-in functions. Provides methods to check entire lists of AST nodes (`check`) and individual nodes (`checkNode`). Handles different types of AST nodes like literals, identifiers, variable declarations, function declarations, and binary expressions.

### Function: check(const std::vector<ASTNodePtr>& nodes)

**Purpose:** Checks an entire list of AST nodes.

**Behavior:** Iterates through each node in the provided vector and calls `checkNode` to validate it against the current environment.

### Function: check(const ASTNodePtr& node)

**Purpose:** Checks a single AST node.

**Behavior:** Depending on whether the node is a block statement or not, it either checks the entire block or just the single node using `checkNode`.

### Function: checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)

**Purpose:** Validates a specific AST node within a given environment.

**Behavior:** Determines the type of the node based on its structure. For example, number literals are considered floats, string literals as strings, and boolean literals as bools. Identifiers resolve their type from the environment, while variable declarations and function parameters enforce type constraints. Binary expressions further refine type validation by comparing the types of their operands.

## Tradeoffs/Limitations

- **Simplified Function Types:** In the implementation of function declarations, the return type is simplified to `"fn"` regardless of the actual return type. This simplification might lead to less precise error messages but avoids complex type systems for quantum functions.
  
- **Limited Error Reporting:** The current implementation primarily reports warnings for type mismatches. More sophisticated error reporting mechanisms could provide clearer guidance but would increase the complexity of the type checker.

- **No Support for Polymorphism:** Due to the focus on static typing, polymorphic features are not supported. This limitation means that explicit type casting must be used where necessary, which could make the code harder to read and maintain.

## Conclusion

`TypeChecker.cpp` plays a vital role in the Quantum Language compiler by ensuring that the AST conforms to the defined types. Through careful design decisions, it provides a reliable foundation for semantic analysis, although some tradeoffs have been made in terms of complexity and feature support.