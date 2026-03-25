# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions involving bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). It leverages the existing parser infrastructure to handle equality expressions and then extends it to incorporate bitwise operations in a sequential manner.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseEquality()` to parse an initial expression. This could be any valid equality expression that might precede a bitwise operation.
2. **Loop for Bitwise Operations**: It enters a loop where it checks if the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, `TokenType::BIT_XOR`). If so, it consumes the operator and recursively calls `parseEquality()` again to parse the right-hand side of the bitwise expression.
3. **Building AST Nodes**: For each bitwise operation encountered, the function constructs an `ASTNode` representing a binary expression. The node contains:
   - The operator value (`op`).
   - A pointer to the parsed left-hand side expression (`std::move(left)`).
   - A pointer to the newly parsed right-hand side expression (`std::move(right)`).
4. **Line Number Tracking**: Each new `ASTNode` includes the line number (`ln`) at which the bitwise operation was found, allowing for better error reporting and debugging.
5. **Return Value**: Once there are no more bitwise operators to process, the function returns the final `ASTNode`.

## Parameters/Return Value
- **Parameters**:
  - None explicitly defined within the function signature; it uses global state managed by the parser, such as `current()`, `consume()`, and `check()`.
  
- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` representing the root of the parsed bitwise expression tree.

## Edge Cases
- **No Bitwise Operators**: If the input expression does not contain any bitwise operators, the function will simply return the result of `parseEquality()`, which could be a single operand or another type of expression.
- **Nested Bitwise Expressions**: The function supports nested bitwise expressions due to its recursive nature. For example, `a & b | c ^ d` would be correctly parsed into a tree structure reflecting the order of operations.
- **Error Handling**: The function relies on the parser's error handling mechanisms. If `parseEquality()` fails to parse an expression correctly, or if unexpected tokens are encountered, the parser will raise appropriate errors.

## Interactions with Other Components
- **Parser Infrastructure**: The function interacts with the broader parsing system through functions like `current()`, `consume()`, and `check()`. These functions manage the stream of tokens and determine their types.
- **AST Construction**: It collaborates with the Abstract Syntax Tree (AST) construction logic to create nodes for bitwise expressions. The `BinaryExpr` struct likely represents a binary operation in the AST.
- **Error Reporting**: By including the line number in each `ASTNode`, the function aids in providing context-specific error messages when issues arise during parsing.

This function is crucial for handling complex expressions in the Quantum Language compiler, ensuring that bitwise operations are correctly interpreted and represented in the AST.