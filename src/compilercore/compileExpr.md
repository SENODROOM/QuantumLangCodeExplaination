# `compileExpr` Function

## Purpose
The `compileExpr` function is a critical component of the Quantum Language compiler responsible for compiling expressions into intermediate representation (IR). It accepts an expression node and its line number as inputs, processing the expression according to its type.

## Parameters
- **node**: The expression node to be compiled.
- **ln**: The line number where the expression occurs in the source code.

## Return Value
This function does not return any value directly but instead emits operations into the IR through calls to `emit`.

## Edge Cases
1. **Nil Literal**: When encountering a `NilLiteral`, the function emits an operation to load a nil constant.
2. **Boolean Literals**: For boolean literals (`True` or `False`), the function emits specific operations (`Op::LOAD_TRUE` or `Op::LOAD_FALSE`) without additional arguments.
3. **Number and String Literals**: These are handled similarly by emitting `Op::LOAD_CONST` followed by the constant value added using `addConst`.
4. **Complex Expressions**: Nested or complex expressions like binary, unary, assignment, function calls, etc., are recursively processed by calling specialized functions (`compileBinary`, `compileUnary`, etc.).

## Interactions with Other Components
- **Emit Function**: `compileExpr` interacts with the `emit` function to generate IR instructions. Each type of expression corresponds to a specific set of IR operations.
- **Constant Pool Management**: The `addConst` function is used to manage the constant pool within the IR, ensuring that each unique constant is only stored once.
- **Expression Visitors**: The use of `std::visit` allows `compileExpr` to handle different types of expression nodes dynamically, making the implementation flexible and scalable.

## Implementation Details
The function uses `std::visit` to dispatch based on the type of the expression node (`n`). Depending on the type, it calls a corresponding helper function:
- **NumberLiteral**: Emits `Op::LOAD_CONST` with the numeric value.
- **StringLiteral**: Emits `Op::LOAD_CONST` with the string value.
- **BoolLiteral**: Emits either `Op::LOAD_TRUE` or `Op::LOAD_FALSE` based on the boolean value.
- **NilLiteral**: Emits `Op::LOAD_NIL`.
- **Identifier**: Calls `compileIdentifier` to handle variable references.
- **BinaryExpr**: Calls `compileBinary` to process binary operations.
- **UnaryExpr**: Calls `compileUnary` to handle unary operations.
- **AssignExpr**: Calls `compileAssign` to process assignments.
- **CallExpr**: Calls `compileCall` to handle function calls.
- **IndexExpr**: Calls `compileIndex` to process indexing operations.
- **SliceExpr**: Calls `compileSlice` to handle slicing operations.
- **MemberExpr**: Calls `compileMember` to process member access operations.
- **ArrayLiteral**: Calls `compileArray` to handle array literals.
- **DictLiteral**: Calls `compileDict` to handle dictionary literals.
- **TupleLiteral**: Calls `compileTuple` to handle tuple literals.
- **LambdaExpr**: Calls `compileLambda` to process lambda expressions.
- **TernaryExpr**: Calls `compileTernary` to handle ternary conditional expressions.
- **ListComp**: Calls `compileListComp` to process list comprehensions.
- **SuperExpr**: Calls `compileSuper` to handle super class references.
- **NewExpr**: Calls `compileNew` to process object instantiation.
- **AddressOfExpr**: Calls `compileAddressOf` to handle address-of operations.

Each of these helper functions is designed to handle specific types of expressions, breaking down the complexity of `compileExpr`. By leveraging `std::visit` and dynamic dispatch, the function ensures efficient and accurate compilation of various expression types in the Quantum Language.