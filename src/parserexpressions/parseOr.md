# `parseOr` Function

## Purpose
The `parseOr` function is essential to the parser in the Quantum Language compiler, responsible for interpreting expressions that involve the logical OR (`or`), logical AND (`&&`), and null-coalescing (`??`) operators. It constructs an Abstract Syntax Tree (AST) representation of these expressions.

## Parameters
- None explicitly defined in the provided code snippet. The function relies on global state such as `pos`, which represents the current position in the input stream, and `current()`, which returns the token at the current position.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed expression. If there are no OR, AND, or null-coalescing operators in the expression, it simply returns the result of parsing an `and` expression using `parseAnd()`.

## How It Works
1. **Parse Left Expression**: The function starts by calling `parseAnd()` to parse the leftmost part of the expression. This part could be a simple value, another binary expression, or any valid sub-expression according to the language's grammar rules.

2. **Loop for OR, AND, and Null-Coalescing Operators**:
   - Inside a loop, the function checks if the next token is one of the following: `TokenType::OR` (logical OR), `TokenType::OR_OR` (logical AND), or `TokenType::NULL_COALESCE` (null-coalescing).
   - If none of these tokens are found, the loop breaks, and the function returns the AST node representing the leftmost expression.
   - If an operator token is found, the function consumes the token and determines its string representation (`"or"` for `TokenType::OR_OR` and the actual token value otherwise).

3. **Skip Newlines**: After consuming the operator token, the function skips any newlines that might follow, ensuring that the parser continues correctly after encountering whitespace.

4. **Parse Right Expression**: The function then recursively calls itself to parse the right-hand side of the current expression, starting from the same point where the operator was consumed.

5. **Construct Binary Expression Node**: Once both the left and right expressions have been parsed, the function creates a new `ASTNode` containing a `BinaryExpr` object. This node encapsulates the operator and the two child nodes (left and right). The line number (`ln`) associated with the current token is also stored in the `BinaryExpr`.

6. **Update Left Node**: The newly constructed `BinaryExpr` node becomes the new left node for the next iteration of the loop, allowing the function to handle multiple consecutive OR, AND, and null-coalescing operations.

7. **Return Final AST Node**: After exiting the loop, the function returns the final AST node, which represents the entire parsed expression including all nested OR, AND, and null-coalescing operations.

## Edge Cases
- **No Operator Found**: If the expression does not contain any OR, AND, or null-coalescing operators, the function will return the result of parsing the `and` expression directly.
- **Whitespace Handling**: The function ensures that any whitespace following an operator is skipped before proceeding to parse the right-hand side of the expression, maintaining correct tokenization and parsing behavior.

## Interactions with Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve the next token and determine its type. This interaction is crucial for identifying whether the current expression contains an OR, AND, or null-coalescing operator.
- **Error Handling**: While not explicitly shown in the provided code snippet, the parser should include error handling mechanisms to manage unexpected tokens or malformed expressions gracefully.
- **Recursive Parsing**: The recursive call to `parseOr()` allows the function to handle nested expressions involving multiple OR, AND, and null-coalescing operators, ensuring that the entire expression is correctly parsed into an AST.

This implementation of the `parseOr` function effectively handles complex expressions involving multiple logical operators, providing a robust foundation for further processing and analysis within the Quantum Language compiler.