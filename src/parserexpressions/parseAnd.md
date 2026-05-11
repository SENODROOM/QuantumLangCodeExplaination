# `parseAnd` Function

## Purpose
The `parseAnd` function is crucial in the Quantum Language compiler's parser for handling expressions involving both the logical AND (`&`) and bitwise AND (`&&`) operators. It constructs an Abstract Syntax Tree (AST) node representing these operations, ensuring correct precedence and associativity during parsing.

## Parameters
- None explicitly listed as parameters; however, it interacts with global state such as `pos`, which tracks the current position in the input source code.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed expression. The node contains a `BinaryExpr` structure, which includes the operator string and pointers to the left and right operands.

## Edge Cases
1. **No Operators**: If there are no `&` or `&&` operators present after the initial call to `parseBitwise()`, the function returns the result of `parseBitwise()` without any modifications.
2. **Mixed Precedence**: The function correctly handles mixed use of logical AND (`&`) and bitwise AND (`&&`). Logical AND has higher precedence than bitwise AND, so the function processes them accordingly.
3. **Comments and Whitespace**: The function skips newlines and consumes tokens until it encounters either `TokenType::AND` or `TokenType::AND_AND`. This ensures that comments and whitespace do not interfere with the parsing process.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. It checks token types using functions like `check()` and consumes tokens using `consume()`.
- **AST Construction**: The function constructs AST nodes using the `std::make_unique` method, creating `BinaryExpr` nodes to represent the parsed expressions.
- **Precedence Handling**: By consuming tokens and recursively calling itself, the function manages operator precedence. It first parses the left operand using `parseBitwise()`, then looks ahead for `&` or `&&` operators before parsing the right operand and combining them into a single `BinaryExpr`.

## Implementation Details
The function starts by parsing the left operand using `parseBitwise()`. It then enters a loop where it repeatedly checks for `&` or `&&` operators. If found, it consumes the operator token, determines its type, and continues parsing the right operand. Each parsed pair of operands and operator is combined into a `BinaryExpr` node, which is then assigned back to `left` for further processing. The loop breaks when no more `&` or `&&` operators are found, ensuring that the function can handle nested expressions correctly.