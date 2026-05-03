# `parseAnd` Function

## Purpose
The `parseAnd` function is integral to the Quantum Language compiler's parser, tasked with interpreting expressions that utilize both the logical AND (`&`) and bitwise AND (`&&`) operators. This function meticulously constructs an Abstract Syntax Tree (AST) node encapsulating the parsed expression, thereby facilitating subsequent semantic analysis and code generation phases.

## Parameters/Return Value
- **Parameters**: None explicitly listed in the provided snippet; however, implicit dependencies include access to the parser's state such as the current token position (`pos`), the ability to check and consume tokens (`check`, `consume`), and the capability to skip newlines (`skipNewlines`).
- **Return Value**: A unique pointer to an ASTNode object, representing the parsed expression. The returned node is constructed using a BinaryExpr structure, which includes the operator type, the left-hand side expression, and the right-hand side expression.

## How It Works
1. **Initial Parsing**: The function begins by invoking `parseBitwise()`, which parses the left-hand side of the AND expression. The result is stored in the variable `left`.

2. **Loop for Additional Operands**: The function enters a loop where it continuously checks for additional occurrences of either the logical AND (`&`) or bitwise AND (`&&`) operators. If such an operator is found:
   - The current position is saved (`savedPos`).
   - Newlines are skipped (`skipNewlines()`).
   - The operator token is consumed (`consume()`), and its type is checked to determine whether it represents a logical AND or a bitwise AND. The corresponding string ("and" or "&&") is stored in `opStr`.
   - Another call to `parseBitwise()` is made to parse the right-hand side of the current AND expression. The result is stored in the variable `right`.
   - An ASTNode is constructed using the `std::make_unique` function, wrapping a `BinaryExpr` object that combines the operator (`opStr`), the previously parsed left-hand side (`std::move(left)`), and the newly parsed right-hand side (`std::move(right)`). The line number of the operator token is also passed to ensure accurate error reporting and debugging.
   - The updated `left` pointer now points to the newly constructed ASTNode, effectively chaining the parsed expressions together.

3. **Termination Condition**: The loop continues until no further AND operators are detected. At this point, the original `left` pointer, which now contains the entire parsed expression chain, is returned.

## Edge Cases
- **No Additional Operators**: If there are no additional AND operators after the initial left-hand side expression, the function simply returns the parsed left-hand side without any modifications.
- **Mixed Operator Usage**: The function can handle mixed usage of logical AND (`&`) and bitwise AND (`&&`) operators, correctly constructing the AST based on the order of operations.
- **Error Handling**: The function implicitly relies on the parser's error handling mechanisms to manage unexpected tokens or malformed expressions. If an invalid token is encountered, the loop terminates, and the parser may report an error.

## Interactions with Other Components
- **Tokenizer**: The function consumes tokens from the tokenizer, relying on its output to identify and process AND operators and operands.
- **SkipNewlines**: This utility function is invoked within the loop to ensure that the parser skips over any extraneous whitespace or newline characters between tokens, maintaining correct token alignment.
- **BinaryExpr Structure**: The function uses the `BinaryExpr` structure to represent binary expressions in the AST. This structure facilitates the combination of multiple expressions into a single, hierarchical representation.
- **Error Reporting**: While not explicitly shown in the snippet, the function indirectly contributes to error reporting by ensuring that each parsed expression is accurately represented in the AST, allowing for precise identification of errors during later stages of compilation.

In summary, the `parseAnd` function plays a crucial role in the Quantum Language compiler's parsing phase, enabling the construction of complex expressions involving AND operators. Its design ensures flexibility and robustness, accommodating various scenarios and interacting seamlessly with other essential components of the compiler.