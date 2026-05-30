# TypeChecker Component

The TypeChecker component is a crucial part of the Quantum Language compiler, responsible for performing static type validation on the Abstract Syntax Tree (AST). This ensures that the code complies with predefined type constraints, thereby avoiding runtime errors due to type mismatches before execution begins.

## Overview

The TypeChecker component processes the AST and validates the types of variables, expressions, and statements according to the language's type system rules. It identifies any type inconsistencies or violations and reports them as errors during the compilation phase.

### Key Files

- **check.cpp**: Contains the implementation of the main type checking logic.
- **check.h**: Defines the public interface for the type checker, including function declarations and data structures used for type validation.
- **checkNode.cpp**: Implements specific type checking routines for different nodes in the AST, such as variable declarations, arithmetic operations, and control flow constructs.

### Flow

1. **Initialization**: The TypeChecker initializes itself and sets up necessary data structures to store type information.
2. **Traversal**: The AST is traversed using a depth-first approach. Each node in the AST is processed based on its type.
3. **Type Validation**: For each node, the TypeChecker applies the appropriate type checking rules. This includes validating the types of operands in binary expressions, ensuring that variables are declared before use, and verifying the correctness of function calls.
4. **Error Reporting**: If any type mismatch or violation is detected, the TypeChecker generates an error message indicating the location and nature of the issue.
5. **Finalization**: After completing the traversal and type validation, the TypeChecker finalizes the type checking process, ensuring all types are correctly resolved and validated.

By following this structured approach, the TypeChecker component effectively enforces type safety in the Quantum Language, enhancing the reliability and robustness of the compiled code.