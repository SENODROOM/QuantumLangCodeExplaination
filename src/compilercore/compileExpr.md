# `compileExpr` Function

## Purpose
The `compileExpr` function is a crucial component of the Quantum Language compiler that compiles expressions into an intermediate representation (IR). This function accepts an expression node and its corresponding line number as inputs, processing the expression based on its type and emitting the appropriate IR instructions.

## Parameters
- `node`: A reference to the expression node that needs to be compiled.
- `ln`: An integer representing the line number in the source code where the expression appears.

## Return Value
This function does not return any value explicitly; instead, it emits IR instructions directly through calls to the `emit` function.

## How It Works
The function uses `std::visit` to dispatch the compilation process based on the type of the expression node. Each case within `std::visit` corresponds to a different type of expression:

1. **NumberLiteral**: Emits an `Op::LOAD_CONST` instruction to load a constant numeric value into the IR.
2. **StringLiteral**: Emits an `Op::LOAD_CONST` instruction to load a constant string value into the IR.
3. **BoolLiteral**: Emits either `Op::LOAD_TRUE` or `Op::LOAD_FALSE` depending on the boolean value.
4. **NilLiteral**: Emits an `Op::LOAD_NIL` instruction to represent a null value.
5. **Identifier**: Calls the `compileIdentifier` function to handle identifier-based expressions.
6. **BinaryExpr**: Calls the `compileBinary` function to handle binary operations like addition, subtraction, etc.
7. **UnaryExpr**: Calls the `compileUnary` function to handle unary operations such as negation.
8. **AssignExpr**: Calls the `compileAssign` function to handle assignment operations.
9. **CallExpr**: Calls the `compileCall` function to handle function calls.
10. **IndexExpr**: Calls the `compileIndex` function to handle indexing operations.
11. **SliceExpr**: Calls the `compileSlice` function to handle slicing operations.
12. **MemberExpr**: Calls the `compileMember` function to handle member access operations.
13. **ArrayLiteral**: Calls the `compileArray` function to handle array literals.
14. **DictLiteral**: Calls the `compileDict` function to handle dictionary literals.
15. **TupleLiteral**: Calls the `compileTuple` function to handle tuple literals.
16. **LambdaExpr**: Calls the `compileLambda` function to handle lambda expressions.
17. **TernaryExpr**: Calls the `compileTernary` function to handle ternary conditional expressions.
18. **ListComp**: Calls the `compileListComp` function to handle list comprehensions.
19. **SuperExpr**: Calls the `compileSuper` function to handle super class references.
20. **NewExpr**: Calls the `compileNew` function to handle object creation expressions.
21. **AddressOfExpr**: Calls the `compileAddressOf` function to handle address-of operations.

Each of these functions (`compileIdentifier`, `compileBinary`, etc.) is designed to handle specific types of expressions and generate the appropriate IR instructions. The `emit` function is used to insert these instructions into the IR stream being built during the compilation process.

## Edge Cases
- **Unknown Expression Type**: If the expression node type is not recognized, the function will likely throw an exception or log an error indicating an unsupported expression type.
- **Empty Literal**: For literal types like empty arrays or dictionaries, special handling may be required to ensure they are correctly represented in the IR.
- **Nested Expressions**: Complex expressions involving nested sub-expressions require proper handling to ensure the correct sequence of operations is emitted in the IR.

## Interactions With Other Components
- **Symbol Table**: The `compileExpr` function interacts with the symbol table to resolve identifiers and determine their types.
- **Type Checker**: Before emitting IR, the function may perform type checking to ensure that the expression adheres to the language's type rules.
- **Error Handler**: The function includes mechanisms for error handling, logging errors when encountering unsupported or invalid expressions.
- **IR Builder**: The `emit` function, which is called internally, is part of the IR builder component, responsible for constructing the IR structure.

In summary, the `compileExpr` function plays a vital role in translating quantum language expressions into IR, ensuring that each type of expression is handled appropriately and efficiently. Its interaction with various compiler components guarantees robust and accurate compilation of quantum programs.