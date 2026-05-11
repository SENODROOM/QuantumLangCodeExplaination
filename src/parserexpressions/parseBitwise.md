# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function builds upon the existing parser infrastructure, which is capable of handling equality expressions, and extends its capabilities to include bitwise operations.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed bitwise expression.

## How It Works
1. **Initialization**: The function begins by calling `parseEquality()` to parse the left-hand side of the bitwise expression. This function returns a unique pointer to an `ASTNode` representing the parsed equality expression.
   
2. **Loop Through Bitwise Operations**: The function enters a loop that continues as long as the next token in the input stream is one of the bitwise operation tokens (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`).

3. **Consume Bitwise Operator**: Inside the loop, the function consumes the current token using the `consume()` method, which advances the tokenizer to the next token and returns the consumed token. The value of the consumed token (i.e., the operator) is stored in the variable `op`.

4. **Parse Right-Hand Side**: The function calls `parseEquality()` again to parse the right-hand side of the bitwise expression. This ensures that the right-hand side is also treated as an equality expression, allowing for nested bitwise operations.

5. **Create Binary Expression Node**: The function creates a new `ASTNode` containing a `BinaryExpr` object, which represents a binary expression. The constructor of `BinaryExpr` takes three arguments:
   - The operator (`op`) as a string.
   - A move-constructed unique pointer to the left-hand side (`std::move(left)`).
   - A move-constructed unique pointer to the right-hand side (`std::move(right)`).
   - The line number (`ln`) where the bitwise operation occurs.

6. **Update Left-Hand Side**: The newly created `ASTNode` is assigned back to the `left` variable, effectively updating the left-hand side of the expression for the next iteration of the loop.

7. **Edge Cases**:
   - If there are no more bitwise operators after the initial call to `parseEquality()`, the function simply returns the parsed equality expression.
   - If the input stream contains invalid tokens or unexpected end-of-file, the tokenizer will raise appropriate errors, causing the parsing process to fail.

8. **Interactions with Other Components**:
   - The `parseBitwise` function relies on the `parseEquality` function to correctly parse the operands of the bitwise operations. This means that `parseBitwise` must be called after `parseEquality` has been defined and implemented.
   - The `consume()` function is used to advance the tokenizer and retrieve the current token's value. This interaction is crucial for correctly identifying and processing the bitwise operators.
   - The `check()` function is used to verify if the next token matches any of the specified bitwise operation types. This allows the function to determine whether to continue parsing bitwise expressions or to exit the loop when no more bitwise operators are found.

By extending the parser infrastructure to handle bitwise operations, the `parseBitwise` function enables the Quantum Language compiler to accurately parse and represent complex expressions involving both equality and bitwise logic. This functionality is essential for supporting advanced programming constructs in the language.