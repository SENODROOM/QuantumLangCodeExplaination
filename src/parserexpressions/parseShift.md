# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts.

## Parameters
- None explicitly mentioned in the code snippet.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed shift expression.

## How It Works
The function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:

1. **Check for Compound Assignment Operators**: If the next token is followed immediately by an assignment operator (`TokenType::ASSIGN`), such as `<<=` or `>>=`, the loop breaks. This is because these operators are handled separately by the `parseAssignment` function.

2. **Consume Current Token**: The function consumes the current token, which should be one of the shift operators (`<<` or `>>`).

3. **Parse Right-Hand Side**: After consuming the shift operator, it calls `parseAddSub()` again to parse the right-hand side of the shift expression.

4. **Create ASTNode**: A new `ASTNode` is created using the `std::make_unique` function. This node represents a binary expression where the type is set to `BinaryExpr`, the operation is stored, and both the left and right operands are moved into the new node.

5. **Update Line Number**: The line number of the current token is recorded and passed to the `ASTNode` constructor to ensure accurate error reporting and debugging.

6. **Return Parsed Expression**: Finally, the function returns the newly created `ASTNode`.

### Edge Cases
- **No Shift Operators**: If there are no shift operators after the initial call to `parseAddSub()`, the function simply returns the result of `parseAddSub()`.
- **Compound Assignment**: As mentioned earlier, the function breaks out of the loop when it encounters a shift operator followed by an assignment operator, ensuring that these are correctly parsed by the `parseAssignment` function.

### Interactions with Other Components
- **`parseAddSub`**: This function is called twice within `parseShift`. The first call parses the left-hand side of the shift expression, and the second call parses the right-hand side. Both functions are part of the same parsing context and use similar grammar rules.
- **`consume`**: This function is used to advance the parser's position to the next token. It takes care of updating the current token and handling any necessary state changes.
- **`check`**: This function checks the type of the current token without advancing the parser's position. It helps determine whether to continue parsing the shift expression or move on to the next part of the language syntax.

In summary, the `parseShift` function is responsible for parsing shift expressions in the Quantum Language compiler. It leverages existing parsing functions (`parseAddSub`, `consume`, `check`) to build up the abstract syntax tree (AST) for these expressions, ensuring that they are correctly identified and represented in the compiled output.