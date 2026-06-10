# `visit` Function

## Purpose
The `visit` function is a template method within the Quantum Language compiler's `CompilerCore` class. Its primary purpose is to process and translate various types of abstract syntax tree (AST) nodes into corresponding quantum machine code instructions. This function serves as a dispatcher that selects the appropriate compilation routine based on the type of AST node encountered during traversal.

## Parameters
- `n`: The AST node to be visited and processed.
- `ln`: The line number where the AST node appears in the source code, used for error reporting and debugging purposes.

## Return Value
This function does not return any value explicitly. Instead, it performs side effects by emitting quantum machine code instructions through the `emit` method.

## Edge Cases
1. **Unknown Node Type**: If the AST node type is not recognized or not handled by any of the conditional checks, the function may lead to undefined behavior or compile errors.
2. **Empty Nodes**: For certain node types like `NilLiteral`, which represent null or empty values, the function ensures that the correct quantum instruction (`Op::LOAD_NIL`) is emitted.
3. **Complex Expressions**: Nodes such as `BinaryExpr` and `UnaryExpr` can involve multiple sub-expressions. The `visit` function recursively processes these sub-expressions before emitting the final quantum instruction.

## Interactions with Other Components
- **Emission of Instructions**: The `emit` method is called within each branch of the `if constexpr` structure to generate quantum machine code instructions. This method interacts with the `MachineCodeBuffer` to store the generated instructions.
- **Constant Pool Management**: The `addConst` method is used to manage constants in the constant pool. It takes a `QuantumValue` and returns an index that can be used to load the constant in the quantum machine code.
- **Compilation Routines**: Each branch of the `if constexpr` structure calls a separate compilation routine (`compileIdentifier`, `compileBinary`, etc.). These routines are responsible for translating specific AST node types into quantum instructions. They interact with other parts of the compiler such as symbol tables, expression evaluators, and code generation logic.
- **Error Handling**: The `ln` parameter is passed to each compilation routine to provide context about the location of the AST node in the source code. This helps in generating meaningful error messages when issues arise during compilation.

## Implementation Details
The implementation uses C++'s `std::is_same_v` and `std::decay_t` to perform compile-time type checking and decay, respectively. This allows the `visit` function to handle different AST node types without runtime overhead, enhancing performance and safety.

Here is a more detailed breakdown of how the function works:

1. **Type Decay and Check**:
   ```cpp
   using T = std::decay_t<decltype(n)>;
   ```
   - `decltype(n)` retrieves the type of the AST node `n`.
   - `std::decay_t` removes any reference qualifiers and cv-qualifiers from the type, ensuring that we work with the underlying type.

2. **Conditional Compilation**:
   ```cpp
   if constexpr (std::is_same_v<T, NumberLiteral>)
       emit(Op::LOAD_CONST, addConst(QuantumValue(n.value)), ln);
   ```
   - `if constexpr` evaluates the condition at compile time. If `T` is `NumberLiteral`, it proceeds to emit the `LOAD_CONST` operation with the constant value converted to a `QuantumValue`.

3. **Handling Different Node Types**:
   - For `StringLiteral`, it emits the `LOAD_CONST` operation similarly but with string data.
   - For `BoolLiteral`, it emits either `LOAD_TRUE` or `LOAD_FALSE` depending on the boolean value.
   - For `NilLiteral`, it directly emits the `LOAD_NIL` operation.
   - For `Identifier`, `BinaryExpr`, `UnaryExpr`, and other complex expressions, it calls specialized methods (`compileIdentifier`, `compileBinary`, etc.) that handle the translation of those node types into quantum instructions.

4. **Recursive Processing**:
   - Some node types, like `BinaryExpr`, require recursive processing of their left and right operands. The `visit` function calls itself with these sub-expressions to ensure they are properly translated.

5. **Error Reporting**:
   - The `ln` parameter is crucial for providing accurate error reporting. If a node type is not supported, the compiler can report an error at the specified line number.

By leveraging C++'s powerful type system and compile-time evaluation capabilities, the `visit` function efficiently translates AST nodes into quantum machine code, ensuring both correctness and performance.