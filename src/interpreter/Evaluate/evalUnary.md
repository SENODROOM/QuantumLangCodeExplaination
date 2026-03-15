# evalUnary Function Explanation

The `evalUnary` function is responsible for evaluating unary expressions in the Quantum Language compiler. Unary expressions consist of an operator and a single operand, such as `-x`, `!y`, or `~z`. This function processes these operators and returns the appropriate result based on the type of operand and the operation specified.

## Parameters

- `UnaryExpr &e`: A reference to the unary expression node that needs to be evaluated. This node contains the operator (`op`) and the operand (`operand`).

## Return Value

- `QuantumValue`: The result of evaluating the unary expression. The type of `QuantumValue` can vary depending on the operand and the operation.

## How It Works

1. **Operand Evaluation**: The function first evaluates the operand using the `evaluate` method. This ensures that the operand is in its simplest form before applying the unary operator.

2. **Operator Handling**:
   - **Negation (`-`)**: If the operator is `-`, the function negates the numeric value of the operand using the `toNum` method. The `toNum` method is used to convert the operand to a number, ensuring that the negation is performed correctly.
   - **Logical Not (`!` or `not`)**: If the operator is either `!` or `not`, the function checks whether the operand is truthy or falsy using the `isTruthy` method. It then returns the logical negation of this truthiness.
   - **Bitwise Not (`~`)**: If the operator is `~`, the function performs a bitwise not operation on the integer value of the operand using the `toInt` method. The result is converted back to a double since `QuantumValue` typically handles numeric values as doubles.
   - **Spread Operator (`...`)**: If the operator is `...`, the function simply returns the operand's value without performing any additional operations. This is useful for handling array spread syntax in quantum programs.

3. **Error Handling**: If an unknown unary operator is encountered, the function throws a `RuntimeError` indicating the unrecognized operator.

## Edge Cases

- **Numeric Overflow**: When performing bitwise operations, there is a risk of numeric overflow. However, the current implementation assumes that the operands are within the range of typical integer types.
- **Type Mismatch**: The `toNum` and `toInt` methods ensure that the operand is converted to the correct type for the operation. If the conversion fails due to a type mismatch, the behavior is undefined and may lead to runtime errors.

## Interactions with Other Components

- **Evaluation Context**: The `evalUnary` function operates within the context of the interpreter, which manages the evaluation of expressions and maintains the state of the program.
- **Expression Tree**: The unary expression node (`UnaryExpr`) is part of the expression tree structure used by the parser to represent the abstract syntax tree of the quantum program. The `evalUnary` function interacts with this tree to retrieve the operand and apply the unary operator.

In summary, the `evalUnary` function is crucial for processing unary expressions in the Quantum Language compiler. It ensures that the operand is properly evaluated and that the unary operator is applied correctly, returning the expected result. The function also includes error handling for unknown operators and type mismatches, maintaining robustness in the interpreter.