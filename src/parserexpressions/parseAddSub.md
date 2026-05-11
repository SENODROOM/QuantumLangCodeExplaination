# `parseAddSub`

## Function Overview

The `parseAddSub` function is a critical component of the Quantum Language compiler's parser, specifically designed to handle arithmetic expressions that involve both addition (`+`) and subtraction (`-`). This function plays a pivotal role in breaking down complex expressions into simpler binary operations.

### Why It Works This Way

The function operates on a recursive descent principle, starting with parsing multiplication and division expressions using `parseMulDiv()`. It then checks for addition or subtraction operators (`TokenType::PLUS` or `TokenType::MINUS`). If such an operator is found, it consumes the operator token, recursively parses another multiplication/division expression, and constructs a new binary expression node. This process repeats until no more addition or subtraction operators are present in the input stream.

This approach ensures that the parser correctly handles expressions with mixed operators, respecting the standard order of operations (multiplication/division before addition/subtraction). By using recursion, the function can handle nested expressions efficiently.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed as parameters within the function signature, but relies on global state managed by the parser (e.g., `current()` and `consume()`).

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` representing the parsed arithmetic expression involving addition and/or subtraction. The `ASTNode` contains a `BinaryExpr` object, which holds the operation type (`"+"` or `"-"`), the left operand, and the right operand.

## Edge Cases

1. **No Operators**: If the input stream does not contain any addition or subtraction operators after the initial call to `parseMulDiv()`, the function simply returns the result of `parseMulDiv()`.

2. **Nested Expressions**: The function correctly handles nested expressions by recursively calling itself. For example, the expression `(3 + 4) * 5 - 6` would be parsed step-by-step as follows:
   - Parse `(3 + 4)` first, resulting in a binary expression node with `"+"` as the operation.
   - Then multiply the result by `5`.
   - Finally, subtract `6` from the product.

3. **Leading Operator**: If an expression starts with an addition or subtraction operator, the function will throw an error because it expects a valid primary expression (handled by `parseMulDiv()`) before encountering an operator.

## Interactions With Other Components

- **`parseMulDiv()`**: This function is called at the beginning of `parseAddSub()` and recursively whenever an addition or subtraction operator is encountered. It is responsible for parsing multiplication and division expressions, forming the base case for the recursive descent.

- **Token Stream Management**: Functions like `current()` and `consume()` manage the token stream. `current()` retrieves the current token without advancing the stream, while `consume()` advances the stream and returns the current token. These functions ensure that the parser processes tokens sequentially and correctly constructs the abstract syntax tree (AST).

- **Error Handling**: If an unexpected token is encountered during parsing, the function throws an error. This helps maintain robustness and correctness in the compilation process.

Overall, `parseAddSub` is essential for accurately parsing arithmetic expressions in the Quantum Language compiler, ensuring that the resulting AST reflects the intended mathematical structure of the code.