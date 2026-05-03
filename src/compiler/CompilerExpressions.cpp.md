# QuantumLanguage Compiler Expressions Module

## Overview

The `src/compiler/CompilerExpressions.cpp` file is a critical component of the QuantumLanguage compiler, responsible for compiling expressions into intermediate representation (IR) instructions that can be executed on the virtual machine (`Vm`). This module handles both binary and unary operations, as well as assignment statements, ensuring that the resulting IR accurately reflects the semantics of the original source code.

### Role in Compiler Pipeline

This module operates within the broader context of the QuantumLanguage compiler's pipeline. It follows the parsing phase where expressions are represented as abstract syntax trees (ASTs). The primary responsibilities include:
- Converting AST nodes representing binary and unary operations into corresponding IR instructions.
- Handling assignment expressions to generate appropriate IR for variable updates.

### Key Design Decisions and Why

#### Binary Operations

The module uses a `std::unordered_map` named `opMap` to map string representations of binary operators to their corresponding IR operations. This approach allows for efficient lookup and ensures that the compiler can handle a wide range of binary operations without extensive conditional logic.

For example, the `compileBinary` function checks if the operator is one of the logical AND or OR operations (`and`, `&&`, `or`, `||`, `??`). If so, it compiles the left operand and emits a jump instruction based on the result. For other operators like arithmetic operations (`+`, `-`, `*`, etc.), it directly maps the operator to its IR counterpart using the `opMap`.

#### Unary Operations

Similarly, unary operations are handled using another `std::unordered_map` named `cops`. This map associates string representations of unary operators with their respective IR operations. The `compileUnary` function uses this map to determine the correct IR operation for each unary expression.

For instance, the module supports negation (`-`) and logical NOT (`!`), among others. Each supported unary operation has a direct mapping to an IR instruction, allowing the compiler to efficiently generate the necessary IR.

#### Assignment Statements

Assignment statements are compiled to update variables with new values. The `compileAssign` function handles both simple assignments (`=`) and compound assignments (`+=`, `-=`, etc.). It normalizes the operator to ensure consistent handling and then generates the appropriate IR instructions.

For compound assignments, the function first compiles the right-hand side expression and then performs the specified operation with the current value of the variable. The result is stored back into the variable.

### Major Classes/Functions Overview

#### Compiler Class

The `Compiler` class contains methods for compiling different types of expressions. These include:
- `compileBinary`: Compiles binary expressions.
- `compileUnary`: Compiles unary expressions.
- `compileAssign`: Compiles assignment expressions.

These functions work together to convert the AST into IR instructions, which are then executed by the virtual machine.

#### Expression Types

The module defines several types of expressions such as `BinaryExpr`, `UnaryExpr`, and `AssignExpr`. Each type represents a specific kind of expression in the source code.

- **BinaryExpr**: Represents binary operations like addition, subtraction, and logical AND.
- **UnaryExpr**: Represents unary operations like negation and bitwise NOT.
- **AssignExpr**: Represents assignment statements, including simple and compound assignments.

### Tradeoffs

While the use of `std::unordered_map` provides efficient lookups for operators, it does require additional memory to store the mappings. Additionally, the module relies on exception handling (`throw std::runtime_error`) to manage cases where unsupported operators are encountered. This approach simplifies error handling but may impact performance in scenarios with frequent errors.

Overall, the `src/compiler/CompilerExpressions.cpp` file plays a vital role in the QuantumLanguage compiler by converting expressions into executable IR instructions. Its design choices balance efficiency, flexibility, and simplicity, making it a robust foundation for further development and optimization.