# `compileExpr` Function

## Purpose
The `compileExpr` function is a crucial method in the Quantum Language compiler, designed to convert expressions into an intermediate representation (IR). This function plays a pivotal role in the compilation process by handling various types of expressions such as literals, identifiers, binary operations, unary operations, assignments, function calls, and more.

## Parameters
- `node`: An expression node representing the expression to be compiled.
- `ln`: The line number where the expression appears in the source code, used for error reporting and debugging purposes.

## Return Value
This function does not explicitly return a value; instead, it generates IR instructions that are stored internally within the compiler. These instructions represent the computation that needs to be performed during execution.

## How It Works
The `compileExpr` function uses a visitor pattern to handle different types of expression nodes. It employs template metaprogramming through `std::visit` and `if constexpr` statements to determine the type of the expression node at compile time. Based on the type, it invokes the appropriate helper function to generate the corresponding IR instructions.

Here's a breakdown of how each type of expression is handled:

1. **NumberLiteral**: Converts the numeric literal into a constant value and emits an `Op::LOAD_CONST` instruction.
2. **StringLiteral**: Converts the string literal into a constant value and emits an `Op::LOAD_CONST` instruction.
3. **BoolLiteral**: Emits either `Op::LOAD_TRUE` or `Op::LOAD_FALSE` based on the boolean value.
4. **NilLiteral**: Emits an `Op::LOAD_NIL` instruction to load a null value.
5. **Identifier**: Delegates to the `compileIdentifier` function to handle variable references.
6. **BinaryExpr**: Delegates to the `compileBinary` function to handle binary operations like addition, multiplication, etc.
7. **UnaryExpr**: Delegates to the `compileUnary` function to handle unary operations like negation, increment, etc.
8. **AssignExpr**: Delegates to the `compileAssign` function to handle assignment operations.
9. **CallExpr**: Delegates to the `compileCall` function to handle function calls.
10. **IndexExpr**: Delegates to the `compileIndex` function to handle indexing operations.
11. **SliceExpr**: Delegates to the `compileSlice` function to handle slicing operations.
12. **MemberExpr**: Delegates to the `compileMember` function to handle member access operations.
13. **ArrayLiteral**: Delegates to the `compileArray` function to handle array literals.
14. **DictLiteral**: Delegates to the `compileDict` function to handle dictionary literals.
15. **TupleLiteral**: Delegates to the `compileTuple` function to handle tuple literals.
16. **LambdaExpr**: Delegates to the `compileLambda` function to handle lambda expressions.
17. **TernaryExpr**: Delegates to the `compileTernary` function to handle conditional ternary operations.
18. **ListComp**: Delegates to the `compileListComp` function to handle list comprehensions.
19. **SuperExpr**: Delegates to the `compileSuper` function to handle super class references.
20. **NewExpr**: Delegates to the `compileNew` function to handle object instantiation.
21. **AddressOfExpr**: Delegates to the `compileAddressOf` function to handle taking the address of an expression.

## Edge Cases
- **Unknown Expression Types**: If the expression type is not recognized, the function should throw an exception indicating an unsupported operation.
- **Error Handling**: Proper error handling is essential, especially when dealing with invalid expressions or syntax errors. The function should report these issues accurately along with their line numbers.

## Interactions with Other Components
- **Symbol Table**: The `compileExpr` function interacts with the symbol table to resolve variables and functions. It retrieves necessary information about symbols to generate correct IR instructions.
- **IR Emitter**: The function uses an IR emitter to generate and store IR instructions. This emitter is responsible for translating high-level expressions into low-level machine instructions.
- **Type Checker**: Before generating IR, the `compileExpr` function may interact with the type checker to ensure that the expression is semantically valid and conforms to the expected data types.

Overall, the `compileExpr` function is a vital part of the Quantum Language compiler, ensuring that all expressions are correctly translated into an executable IR format. Its use of template metaprogramming and the visitor pattern makes it highly flexible and capable of handling a wide range of expression types efficiently.