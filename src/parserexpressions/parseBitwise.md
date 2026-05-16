# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function builds upon the existing parser infrastructure, which is capable of handling equality expressions, and extends it to support additional bitwise operations.

## Parameters
- None explicitly mentioned in the code snippet provided.

## Return Value
- Returns a unique pointer to an ASTNode representing the parsed bitwise expression.

## How It Works
1. **Initialization**: The function starts by calling `parseEquality()` to get the left-hand side of the bitwise expression. This initial call assumes that the parser has already consumed any leading tokens that could be part of an equality expression.

2. **Loop for Bitwise Operations**: The function then enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`). Inside the loop:
   - It records the current line number using `current().line`.
   - It consumes the current token to get the operator (`op`) and stores its value.
   - It recursively calls `parseEquality()` again to get the right-hand side of the bitwise operation.
   - It creates a new `ASTNode` with a `BinaryExpr` containing the operator, the left-hand side, and the right-hand side. The `BinaryExpr` represents a binary expression node in the abstract syntax tree (AST).
   
3. **Update Left Node**: The newly created `ASTNode` becomes the new left-hand side for the next iteration of the loop, allowing the function to handle multiple consecutive bitwise operations.

4. **Exit Loop**: Once there are no more bitwise operators following the current expression, the loop exits, and the function returns the final `left` ASTNode, which now represents the entire bitwise expression.

5. **Edge Cases**:
   - If the input stream contains no bitwise operators after the initial equality expression, the function will simply return the result of `parseEquality()`.
   - If the input stream contains invalid tokens or unexpected characters, the parser's error handling mechanisms will be triggered, potentially resulting in a parsing error.

6. **Interactions with Other Components**:
   - This function interacts with the `consume()` method, which advances the parser's token stream and retrieves the current token.
   - It also interacts with the `check()` method, which checks if the current token matches a specified type without consuming it.
   - The `parseBitwise()` function relies on the `parseEquality()` function to correctly parse the operands of the bitwise operations, ensuring that the AST accurately reflects the structure of the input expression.

By extending the parser's capabilities to include bitwise operations, the `parseBitwise` function allows for a more comprehensive understanding and representation of quantum language expressions within the compiler's AST. This enables subsequent stages of compilation to perform accurate optimizations and transformations based on the bitwise logic present in the source code.