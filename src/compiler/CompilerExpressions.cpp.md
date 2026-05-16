# QuantumLanguage Compiler Expressions Module

## Overview

The `src/compiler/CompilerExpressions.cpp` file is a vital part of the QuantumLanguage compiler, focusing on the compilation of expressions into Intermediate Representation (IR) instructions suitable for execution on the Virtual Machine (`Vm`). This module manages both binary and unary operations, as well as assignment statements, ensuring efficient and accurate compilation of quantum language expressions.

### Role in Compiler Pipeline

This module operates within the compiler's expression handling phase. It takes parsed expressions and converts them into IR instructions that can be executed by the Vm. The output of this module serves as input to subsequent phases such as optimization and code generation.

### Key Design Decisions and Why

1. **Binary Operations Handling**: 
   - The module uses a switch-like structure (`std::unordered_map`) to map binary operators to their corresponding IR operations. This approach enhances readability and maintainability by centralizing operator mappings in one place.
   
2. **Conditional Compilation**:
   - For conditional operators like `and`, `or`, and `??`, the module emits jump instructions based on the evaluation of the left operand. If the condition is met, it jumps to the right operand; otherwise, it continues with the next instruction. This ensures that only necessary parts of the expression are compiled, optimizing performance.

3. **Assignment Statements**:
   - The module supports both simple assignments (`=`) and compound assignments (`+=`, `-=`). For compound assignments, it normalizes the operator and performs the appropriate sequence of IR instructions. This allows for a unified handling of different types of assignments.

4. **Error Handling**:
   - The module includes robust error handling mechanisms to manage unknown operators. If an unrecognized operator is encountered, it throws a runtime exception, preventing the compiler from proceeding with incorrect or incomplete code.

### Major Classes/Functions Overview

1. **`Compiler::compileBinary(BinaryExpr &e, int line)`**:
   - Compiles binary expressions into IR instructions.
   - Handles logical operators (`and`, `or`, `??`), comparison operators, arithmetic operators, and bitwise operators.
   - Uses a `std::unordered_map` to map operators to their corresponding IR operations.

2. **`Compiler::compileUnary(UnaryExpr &e, int line)`**:
   - Compiles unary expressions into IR instructions.
   - Supports negation (`-`), logical not (`!`, `not`), bitwise not (`~`), increment (`++`), and decrement (`--`).
   - Emits the appropriate IR operation based on the unary operator.

3. **`Compiler::compileAssign(AssignExpr &e, int line)`**:
   - Compiles assignment expressions into IR instructions.
   - Handles both simple and compound assignments.
   - Normalizes compound operators and performs the necessary sequence of IR instructions.

### Tradeoffs

1. **Readability vs. Performance**:
   - Using a `std::unordered_map` for operator mapping improves readability but may slightly impact performance due to hash lookups.

2. **Flexibility vs. Complexity**:
   - Supporting both simple and compound assignments adds flexibility but increases the complexity of the codebase.

3. **Error Handling vs. Robustness**:
   - Throwing exceptions for unknown operators provides robustness but can lead to less user-friendly error messages compared to returning error codes.

Overall, the `src/compiler/CompilerExpressions.cpp` file plays a crucial role in the QuantumLanguage compiler by efficiently converting expressions into executable IR instructions. Its design decisions balance readability, performance, flexibility, and robustness, making it a well-engineered component of the compiler.