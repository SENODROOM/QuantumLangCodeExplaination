# `parseAnd` Function

## Purpose
The `parseAnd` function is crucial in the Quantum Language compiler's parser for handling expressions involving both the logical AND (`&`) and bitwise AND (`&&`) operators. This function builds an Abstract Syntax Tree (AST) node to represent these operations.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed expression.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseBitwise()` to parse the left-hand side of the expression. This initial parsing sets up the first operand for the AND operation.

2. **Loop for Subsequent Operations**: 
   - The function enters a loop where it continuously checks for subsequent AND or bitwise AND operators.
   - Inside the loop:
     - It saves the current position (`savedPos`) to allow backtracking if necessary.
     - It skips any new lines using `skipNewlines()`.
     - It checks if the next token is either `TokenType::AND` or `TokenType::AND_AND`. If not, it breaks out of the loop, indicating there are no more AND operations to parse.
     - It records the line number (`ln`) of the current token.
     - It consumes the current token (`opToken`) which could be either `&` or `&&`, and determines the operator string (`opStr`) based on its type.
     - It again skips any new lines using `skipNewlines()`.
     - It recursively calls `parseBitwise()` to parse the right-hand side of the current AND operation.
     - It creates a new `ASTNode` with a `BinaryExpr` containing the operator string, the left operand, and the right operand. The line number (`ln`) is also passed to maintain context in the AST.

3. **Edge Cases**:
   - The function handles expressions without any AND or bitwise AND operators gracefully by breaking out of the loop when no such tokens are found.
   - If the current token is neither `&` nor `&&`, the function restores the original position (`pos = savedPos`) and exits the loop, ensuring that only valid AND expressions are processed.

4. **Interactions with Other Components**:
   - The `parseAnd` function interacts with the `parseBitwise()` function, which is responsible for parsing bitwise operations. By calling `parseBitwise()` twice within the loop, it ensures that each part of the AND expression is correctly parsed.
   - The `skipNewlines()` function is used to handle potential whitespace between tokens, allowing for better readability and flexibility in the input code.
   - The `consume()` function retrieves the current token and advances the parser to the next token, enabling the function to identify and process different types of operators.

Overall, the `parseAnd` function plays a vital role in constructing complex expressions involving AND operations in the Quantum Language compiler's parser. Its design allows for easy extension and maintenance, making it a robust component of the compiler's syntax analysis phase.