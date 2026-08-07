# `parseAnd` Function

## Purpose
The `parseAnd` function is essential in the Quantum Language compiler's parser for interpreting expressions involving both the logical AND (`&`) and bitwise AND (`&&`) operators. This function constructs an Abstract Syntax Tree (AST) node to accurately represent these operations, ensuring proper evaluation during compilation.

## Parameters/Return Value
- **Parameters**: None explicitly defined in the provided code snippet, but it relies on global variables like `pos`, which tracks the current position in the input source code, and `current()`, which returns the token at the current position.
- **Return Value**: The function returns a unique pointer to an `ASTNode` representing the parsed expression. If there are no AND or BITWISE AND operators present, it simply returns the result of parsing the bitwise expression using `parseBitwise()`.

## How It Works
1. **Parsing Bitwise Expression**: The function begins by calling `parseBitwise()` to parse the initial bitwise expression. This sets up the `left` variable with the root of the AST subtree corresponding to the bitwise operation.

2. **Handling Multiple AND/BITWISE AND Operators**: The function enters a loop where it continuously checks for the presence of either the logical AND (`&`) or bitwise AND (`&&`) operator. If such an operator is found:
   - It saves the current position (`savedPos`) to allow backtracking if necessary.
   - It skips any newlines following the operator.
   - It consumes the operator token (`opToken`) and determines whether it was a logical AND or bitwise AND based on its type.
   - It parses another bitwise expression using `parseBitwise()` and stores it in the `right` variable.
   - It creates a new `ASTNode` with a `BinaryExpr` containing the operator string (`opStr`), the previously parsed `left` subtree, and the newly parsed `right` subtree. The line number (`ln`) of the operator is also recorded.
   - It updates the `left` variable with the newly constructed `ASTNode`.

3. **Loop Termination**: The loop continues until an operator is not found, at which point the original `left` subtree is returned as the complete parsed expression.

## Edge Cases
- **No AND/BITWISE AND Operators**: If the input expression does not contain any AND or BITWISE AND operators, the function will only call `parseBitwise()` once and return its result directly.
- **Backtracking**: In case an unexpected token is encountered after consuming an AND or BITWISE AND operator, the function will backtrack to the saved position before attempting to parse the next part of the expression.

## Interactions with Other Components
- **Tokenizer**: The function uses tokens obtained from the tokenizer to identify and consume AND and BITWISE AND operators.
- **ParseBitwise Function**: `parseAnd` calls `parseBitwise()` to handle the parsing of individual bitwise expressions, forming subtrees within the overall AST.
- **Error Handling**: While not shown in the provided snippet, the function likely interacts with error handling mechanisms to manage syntax errors related to AND and BITWISE AND operators.

By understanding how `parseAnd` functions, developers can better grasp the structure and evaluation of expressions involving these operators within the Quantum Language compiler.