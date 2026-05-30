# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function builds upon an existing parser infrastructure for equality expressions, extending its capabilities to include handling of bitwise operators.

## Parameters/Return Value
- **Parameters**: None explicitly listed in the provided code snippet; however, it relies on global variables or functions within the same scope to access the current token and perform checks and consumption.
- **Return Value**: The function returns a unique pointer to an ASTNode representing the parsed expression. This node can be either a simple equality expression or a binary expression combining equality expressions with bitwise operators.

## How It Works
1. **Initialization**: The function starts by parsing the left-hand side of the bitwise expression using the `parseEquality()` function. This function call retrieves the first operand of the bitwise operation.
2. **Loop Through Bitwise Operators**: The function then enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, `TokenType::BIT_XOR`). Inside the loop:
   - **Line Number**: The line number of the current token is captured and stored in the variable `ln`.
   - **Operator Consumption**: The current token (which is a bitwise operator) is consumed using the `consume()` function, and its value (e.g., `"&"`) is retrieved and stored in the variable `op`.
   - **Right Hand Side Parsing**: Another call to `parseEquality()` is made to parse the right-hand side of the bitwise expression, retrieving the second operand.
   - **Binary Expression Construction**: A new `BinaryExpr` object is constructed with the operator `op`, the previously parsed left-hand side (`left`), and the newly parsed right-hand side (`right`). This new binary expression becomes the new left-hand side for the next iteration of the loop.
3. **Loop Exit**: Once there are no more bitwise operators following the current expression, the loop exits, and the final parsed expression (now stored in `left`) is returned.

## Edge Cases
- **No Bitwise Operations**: If the input expression does not contain any bitwise operators, the function will simply return the result of `parseEquality()`, which could be a single equality expression or a combination of equality expressions without any modifications.
- **Nested Bitwise Expressions**: The function handles nested bitwise expressions correctly by recursively calling itself. For example, the expression `(a & b) | c` would be parsed as follows:
  - First, `(a & b)` is parsed as a binary expression.
  - Then, `| c` is parsed as another binary expression combining the previous result with `c`.

## Interactions With Other Components
- **Parser Infrastructure**: The `parseBitwise` function interacts with the parser infrastructure by leveraging the `parseEquality()` function. This means that any changes or enhancements made to the `parseEquality` function will also affect how `parseBitwise` parses expressions.
- **Token Stream**: The function uses the global token stream to check and consume tokens. This interaction ensures that the parser processes tokens in sequence according to their appearance in the source code.
- **Error Handling**: While not shown in the provided code snippet, the function likely integrates with the error handling mechanisms of the parser. If a syntax error occurs during the parsing of a bitwise expression, appropriate error messages should be generated and propagated up the call stack.

In summary, the `parseBitwise` function efficiently extends the parser's capabilities to handle bitwise operations, building upon an existing foundation for equality expressions. Its design allows for easy integration with other parts of the parser and robust handling of various edge cases.