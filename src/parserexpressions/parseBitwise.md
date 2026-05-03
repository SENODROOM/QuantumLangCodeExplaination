# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions involving bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). It leverages the existing parser infrastructure to handle equality expressions and then extends it to incorporate bitwise operators.

## Parameters
- None explicitly listed in the provided code snippet.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed bitwise expression.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseEquality()` to parse the initial expression. This could be a simple equality expression like `a == b`.

2. **Loop for Bitwise Operations**: 
   - The function enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`).
   - Inside the loop:
     - It records the current line number using `current().line`.
     - It consumes the current token (which is the bitwise operator) using `consume()`. The consumed token's value (e.g., `"&"`) is stored in the variable `op`.
     - It recursively calls `parseEquality()` again to parse the right-hand side of the bitwise operation.
     - It constructs a new `ASTNode` with a `BinaryExpr` containing the operator `op`, the previously parsed left-hand side (`left`), and the newly parsed right-hand side (`right`). The line number is passed to the `ASTNode` constructor.
     - This new node becomes the new left-hand side for the next iteration of the loop.

3. **Exit Loop**: Once there are no more bitwise operators following the current expression, the loop exits, and the final parsed expression (stored in `left`) is returned.

## Edge Cases
- **No Bitwise Operators**: If the input expression does not contain any bitwise operators, the function will simply return the result of `parseEquality()`, which could be a single variable or a more complex expression.
- **Nested Bitwise Expressions**: The function can handle nested bitwise expressions correctly due to its recursive nature. For example, `a & b | c` would be parsed as `(a & b) | c`.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. It checks the type of the current token against `TokenType::BIT_AND`, `TokenType::BIT_OR`, and `TokenType::BIT_XOR` to determine if a bitwise operation should be parsed.
- **Parser Infrastructure**: `parseBitwise` builds upon the existing parser infrastructure, specifically the `parseEquality` function, which handles simpler equality expressions. By extending this infrastructure, `parseBitwise` can parse more complex expressions involving bitwise operations.
- **AST Construction**: The function constructs an Abstract Syntax Tree (AST) using `std::make_unique<ASTNode>` where each node represents either a simple expression or a binary expression combining two sub-expressions with a bitwise operator. This AST is used for further compilation and execution phases.

In summary, the `parseBitwise` function efficiently parses expressions involving bitwise operations by leveraging the existing parser infrastructure and extending it to handle these additional operators. Its ability to handle both simple and nested expressions makes it a robust component of the Quantum Language compiler.