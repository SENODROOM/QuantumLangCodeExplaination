# `parseAnd` Function

## Purpose
The `parseAnd` function is essential in the Quantum Language compiler's parser for managing expressions that include both the logical AND (`&`) and bitwise AND (`&&`) operators. This function constructs an Abstract Syntax Tree (AST) node representing these operations, ensuring correct precedence and associativity.

## Parameters
- None explicitly listed in the provided code snippet.

## Return Value
- The function returns a unique pointer to an `ASTNode`, which represents the parsed expression tree.

## How It Works
1. **Initialization**: The function starts by calling `parseBitwise()`, which parses the left-hand side of the expression. This initial parsing sets up the first operand of the AND operation.

2. **Loop for Additional Operands**: The function enters a loop that continues until there are no more AND or bitwise AND operators in the input stream.
   - Inside the loop:
     - It saves the current position using `savedPos`.
     - Skips any newlines using `skipNewlines()`.
     - Checks if the next token is either `TokenType::AND` or `TokenType::AND_AND`. If not, it restores the saved position and breaks out of the loop, indicating that no further AND operations are present.
     - Consumes the current token (either `&` or `&&`) and stores its line number (`ln`). It then determines whether the consumed token was `&` or `&&` and assigns the corresponding string representation (`"and"` or `"&&"`) to `opStr`.
     - Skips any newlines again after consuming the operator token.
     - Recursively calls `parseBitwise()` to parse the right-hand side of the current AND operation.
     - Constructs a new `ASTNode` containing a `BinaryExpr` object, which encapsulates the operator (`opStr`), the left-hand side (`left`), and the right-hand side (`right`). The line number (`ln`) is also passed to ensure accurate error reporting.
     - Updates the `left` variable to point to the newly constructed AST node, preparing it for the next iteration of the loop.

3. **Edge Cases**:
   - The function correctly handles expressions where multiple AND or bitwise AND operators are chained together. For example, `a & b && c & d` would be parsed as `(a & b) && (c & d)`.
   - If the end of the input stream is reached without encountering any AND or bitwise AND operators, the function simply returns the initially parsed left-hand side operand.
   - The function ensures that only valid tokens are considered for AND operations, skipping over any invalid or unexpected tokens.

4. **Interactions with Other Components**:
   - The `parseAnd` function interacts closely with the `parseBitwise` function, which is responsible for parsing bitwise operations. This interaction allows the parser to build complex expressions involving both logical and bitwise AND operations.
   - The function also relies on the `skipNewlines` and `consume` functions to handle whitespace and advance through the input stream, respectively. These functions contribute to maintaining the integrity of the parsed expression and accurately tracking the source code's line numbers.

In summary, the `parseAnd` function plays a critical role in the Quantum Language compiler's parser by constructing an AST node for expressions involving both logical and bitwise AND operations. Its design ensures correct parsing and precedence handling, making it an integral part of the compiler's functionality.