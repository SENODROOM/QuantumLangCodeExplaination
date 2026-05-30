# `parseAnd` Function

## Purpose
The `parseAnd` function plays a pivotal role in the Quantum Language compiler's parser by handling expressions that involve the logical AND (`&`) and bitwise AND (`&&`) operators. It constructs an Abstract Syntax Tree (AST) node to accurately represent these operations.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an ASTNode representing the parsed expression.

## How It Works
1. **Initialization**: The function starts by calling `parseBitwise()` to parse the left-hand side of the expression. This initial parsing sets up the first operand.
2. **Loop**: The function enters a loop where it repeatedly checks for either the logical AND (`&`) or bitwise AND (`&&`) operator.
   - If such an operator is found, the function consumes it using `consume()`, which advances the parser's position.
   - Depending on whether the consumed token is `TokenType::AND_AND` (bitwise AND) or `TokenType::AND` (logical AND), it determines the operation string (`"and"` or `"&&"`).
3. **Right Operand Parsing**: After consuming the operator, the function calls `parseBitwise()` again to parse the right-hand side of the expression.
4. **Node Construction**: The function then creates a new ASTNode of type `BinaryExpr`. This node encapsulates the operation string along with the left and right operands as unique pointers.
5. **Edge Cases Handling**: 
   - If the next token is neither `TokenType::AND` nor `TokenType::AND_AND`, the function breaks out of the loop, indicating the end of the `&` and `&&` sequence.
   - The function also handles potential errors gracefully by saving the current position (`savedPos`) before checking for the operators. If the check fails, it restores the position (`pos = savedPos`), ensuring that subsequent parsing can continue without interruption.
6. **Interactions with Other Components**:
   - The `parseBitwise()` function is called multiple times within the loop, allowing for nested expressions involving bitwise AND.
   - The `skipNewlines()` function ensures that any whitespace or newline characters between tokens are ignored, maintaining the integrity of the parsed structure.

## Detailed Explanation
- **Initial Parsing**: The function begins by parsing the left-hand side of the expression using `parseBitwise()`. This method returns a unique pointer to an ASTNode representing the left operand.
  
  ```cpp
  auto left = parseBitwise();
  ```

- **Operator Loop**: The core of the function is a loop that continues until it encounters a token that is not an `&` or `&&`. Within each iteration of the loop:
  - The current position is saved (`savedPos`) to allow for error recovery.
  - Whitespace and newline characters are skipped using `skipNewlines()`.
  - The function checks for the presence of either `TokenType::AND` or `TokenType::AND_AND`. If neither is found, the loop exits, and the function returns the parsed left-hand side.
  
  ```cpp
  while (true)
  {
      size_t savedPos = pos;
      skipNewlines();
      if (!check(TokenType::AND) && !check(TokenType::AND_AND))
      {
          pos = savedPos;
          break;
      }
  ```

- **Consuming Operator**: If an operator is found, the function consumes it using `consume()`. This method updates the parser's state to reflect the consumption of the token.
  
  ```cpp
  auto opToken = consume(); // eat 'and' or '&&'
  ```

- **Determining Operation String**: Based on the type of the consumed token, the function determines the appropriate operation string (`"and"` or `"&&"`). This string is used later to construct the ASTNode.
  
  ```cpp
  std::string opStr = (opToken.type == TokenType::AND_AND) ? "and" : opToken.value;
  ```

- **Skipping Newlines Again**: After determining the operation string, the function skips any additional whitespace or newline characters.
  
  ```cpp
  skipNewlines();
  ```

- **Parsing Right Operand**: The function then parses the right-hand side of the expression using `parseBitwise()`, similar to how the left-hand side was parsed initially.
  
  ```cpp
  auto right = parseBitwise();
  ```

- **Constructing ASTNode**: Finally, the function constructs a new ASTNode of type `BinaryExpr`. This node contains the operation string, the left operand, and the right operand. The line number of the current token is passed to ensure accurate location information in the AST.
  
  ```cpp
  left = std::make_unique<ASTNode>(BinaryExpr{opStr, std::move(left), std::move(right)}, ln);
  ```

By following this structured approach, the `parseAnd` function effectively handles complex expressions involving both logical and bitwise AND operations, constructing a robust AST that reflects the intended semantics of the code.