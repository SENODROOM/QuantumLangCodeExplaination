# `compileUnary` Function

## Purpose
The `compileUnary` function is designed to handle the compilation of unary operations in the Quantum Language. Unary operations include negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement operators (`++`, `--`). This function ensures that each unary operation is correctly translated into corresponding quantum machine code instructions.

## Parameters
- `e`: A reference to an `Expression` object representing the unary expression to be compiled. The `Expression` object contains information about the operator (`op`) and its operand.

## Return Value
- None. The function directly emits quantum machine code instructions using the `emit` function.

## Edge Cases
- **Unknown Operator**: If the unary operator encountered is not recognized (`-`, `!`, `not`, `~`, `++`, or `--`), the function throws a `std::runtime_error`.
- **Non-Identifier Operand**: For increment (`++`) and decrement (`--`) operators, if the operand is not an identifier (a variable name), the function will not store the result back into the operand, which could lead to unexpected behavior if the operand is used elsewhere in the program.

## Interactions with Other Components
- **`compileExpr` Function**: Before handling the unary operation, the function calls `compileExpr` on the operand to ensure that the operand itself is properly compiled.
- **`emit` Function**: This function is crucial as it translates the unary operation into quantum machine code instructions. It takes three arguments:
  - An `Op` enum value indicating the type of quantum instruction.
  - A constant value (if applicable) to be loaded or operated upon.
  - The current line number for error reporting and debugging purposes.
- **`addConst` Function**: When compiling the increment (`++`) and decrement (`--`) operators, the function uses `addConst` to create a constant quantum value representing the number `1.0`. This constant is then loaded onto the stack before performing the addition or subtraction.
- **`emitStore` Function**: For the increment and decrement operators when applied to identifiers, after computing the new value, the function uses `emitStore` to store the updated value back into the specified identifier.
- **`emitPop` Function**: After storing the updated value back into the identifier, the function uses `emitPop` to remove the temporary duplicate value from the stack.

## Detailed Explanation
The `compileUnary` function begins by calling `compileExpr` on the operand to ensure that the operand is fully compiled. Depending on the unary operator (`op`) encountered, the function performs different actions:

- **Negation (`-`)**: The function emits the `NEG` instruction to negate the value on top of the stack.
  
- **Logical Not (`!`, `not`)**: The function emits the `NOT` instruction to perform a logical not operation on the value on top of the stack. Both `!` and `not` are treated equivalently.

- **Bitwise Not (`~`)**: The function emits the `BIT_NOT` instruction to perform a bitwise not operation on the value on top of the stack.

- **Increment (`++`)**: 
  - The function first loads the constant `1.0` onto the stack using `emitLoadConst`.
  - Then, it adds this constant to the value on top of the stack using the `ADD` instruction.
  - If the operand is an identifier (variable), the function duplicates the value (to preserve the original value before incrementing), stores the incremented value back into the identifier using `emitStore`, and finally pops the temporary duplicate value from the stack using `emitPop`.

- **Decrement (`--`)**:
  - Similar to the increment case, the function first loads the constant `1.0` onto the stack.
  - However, instead of adding, it subtracts this constant from the value on top of the stack using the `SUB` instruction.
  - If the operand is an identifier, the function duplicates the value, decrements the value, stores it back into the identifier, and pops the temporary duplicate value from the stack.

If the unary operator is none of the above, the function raises an exception indicating an unknown unary operation.

This function is integral to the Quantum Language compiler as it handles the translation of unary expressions into quantum machine code, ensuring correct execution of these operations within the quantum environment.