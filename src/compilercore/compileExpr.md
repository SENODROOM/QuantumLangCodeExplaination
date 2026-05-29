# `compileExpr` Function

## Purpose
The `compileExpr` function serves as a central component within the Quantum Language compiler, responsible for converting expressions into an intermediate representation (IR). By accepting an expression node and its corresponding line number as inputs, this function processes various types of expressions and emits appropriate IR instructions based on their nature.

## Parameters
- **node**: A reference to the expression node that needs to be compiled.
- **ln**: An integer representing the line number in the source code where the expression occurs, used for error reporting and debugging purposes.

## Return Value
This function returns void. It directly modifies the IR output through calls to the `emit` function, which adds instructions to the IR.

## Edge Cases
1. **Empty Expressions**: If the input expression node is empty or null, the function should handle this gracefully without emitting any IR instructions.
2. **Unsupported Literals**: The function currently supports only basic literals like numbers, strings, booleans, and nil. Any unsupported literal types should result in an error during compilation.
3. **Complex Expressions**: For complex expressions involving multiple nested sub-expressions, the function must correctly evaluate each part and combine them into a single IR instruction.

## Interactions with Other Components
- **IR Emission**: The primary interaction is with the IR emission mechanism, specifically through the `emit` function. Each type of expression corresponds to a different set of IR operations, such as loading constants, handling variables, performing arithmetic operations, etc.
  
- **Expression Visitors**: The function uses `std::visit` to dispatch to different visitor functions based on the type of expression node (`NumberLiteral`, `StringLiteral`, `BoolLiteral`, etc.). These visitors encapsulate the logic for compiling each specific type of expression into IR.

- **Error Handling**: During the compilation process, the function may encounter errors related to invalid syntax or unsupported features. It interacts with the error handling subsystem to report these issues accurately.

- **Symbol Table Management**: When dealing with identifiers, the function interacts with the symbol table to resolve variable names and retrieve their corresponding IR values.

Here's a more detailed breakdown of how the function handles different types of expressions:

### Literal Types
For literal types such as numbers, strings, booleans, and nil:
- **NumberLiteral**: Emits an `Op::LOAD_CONST` instruction with the numeric value converted to a `QuantumValue`.
- **StringLiteral**: Similarly, emits an `Op::LOAD_CONST` instruction but with string data.
- **BoolLiteral**: Emits either `Op::LOAD_TRUE` or `Op::LOAD_FALSE` depending on the boolean value.
- **NilLiteral**: Emits an `Op::LOAD_NIL` instruction to represent the null value.

### Compound Expressions
For compound expressions like binary and unary operators:
- **BinaryExpr**: Calls `compileBinary` to handle the left and right operands, then combines them with the appropriate binary operation opcode.
- **UnaryExpr**: Calls `compileUnary` to handle the operand and applies the unary operation opcode.

### Control Flow Expressions
For control flow expressions such as assignments and ternary conditions:
- **AssignExpr**: Calls `compileAssign` to handle the assignment logic, typically involving loading a value and storing it in a variable.
- **TernaryExpr**: Calls `compileTernary` to evaluate the condition and select between two possible outcomes.

### Data Structures
For data structures like arrays, dictionaries, tuples, and lists:
- **ArrayLiteral**: Calls `compileArray` to handle the array elements and construct an array object.
- **DictLiteral**: Calls `compileDict` to handle dictionary key-value pairs and construct a dictionary object.
- **TupleLiteral**: Calls `compileTuple` to handle tuple elements and construct a tuple object.
- **ListComp**: Calls `compileListComp` to handle list comprehensions and generate the appropriate IR for the resulting list.

### Advanced Features
For advanced features like lambda expressions and super calls:
- **LambdaExpr**: Calls `compileLambda` to convert the lambda function into an IR callable object.
- **SuperExpr**: Calls `compileSuper` to handle super method calls, resolving the correct base class and method.

### Error Reporting
If the function encounters an unsupported expression type or a syntax error, it reports an error using the provided line number (`ln`). This ensures that developers can easily identify and fix issues in their source code.

In summary, the `compileExpr` function is essential for translating quantum language expressions into an executable IR format. Its design leverages modern C++ features like `std::visit` to efficiently handle various expression types while ensuring robust error handling and integration with other compiler components.