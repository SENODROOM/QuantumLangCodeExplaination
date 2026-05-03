# `parseOr` Function

## Purpose
The `parseOr` function is part of the parser in the Quantum Language compiler and is responsible for parsing expressions involving logical OR (`or`), logical AND (`&&`), and null-coalescing (`??`) operators. It constructs an Abstract Syntax Tree (AST) node representing these operations.

## Parameters
- None explicitly listed as parameters, but implicitly uses global variables such as `pos`, which represents the current position in the input source code.

## Return Value
- Returns a unique pointer to an `ASTNode` object that represents the parsed expression tree involving the logical OR, logical AND, and null-coalescing operators.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseAnd()` to parse the left-hand side of the expression. This function handles the parsing of expressions involving the logical AND (`&&`) operator.
2. **Loop for Multiple Operations**: 
   - A loop is initiated to handle multiple consecutive logical OR (`or`), logical AND (`&&`), and null-coalescing (`??`) operators.
   - Inside the loop, it saves the current position (`savedPos`) and skips any newlines using `skipNewlines()`.
3. **Check for Operators**:
   - The function checks if the next token is one of the specified operators (`TokenType::OR`, `TokenType::OR_OR`, `TokenType::NULL_COALESCE`). If not, it breaks out of the loop, restoring the previous position (`pos = savedPos`).
4. **Consume Operator Token**:
   - If an operator is found, it consumes the token using `consume()` and stores its line number (`ln`).
   - Depending on the type of operator consumed, it constructs a string representation (`opStr`) of the operator. For example, `TokenType::OR_OR` is converted to `"or"`.
5. **Skip Newlines Again**: After consuming the operator token, it skips any newlines again using `skipNewlines()`.
6. **Recursive Parsing**: The function recursively calls itself (`parseAnd()`) to parse the right-hand side of the current binary operation.
7. **Construct AST Node**: It constructs a new `ASTNode` with a `BinaryExpr` containing the operator string (`opStr`), the left-hand side (`std::move(left)`), and the right-hand side (`std::move(right)`). The line number (`ln`) is also passed to maintain context within the AST.
8. **Update Left Hand Side**: The newly constructed `ASTNode` becomes the new left-hand side for the next iteration of the loop, allowing for chaining of multiple operations.

## Edge Cases
- **No Operators Found**: If there are no more logical OR, logical AND, or null-coalescing operators following the initial call to `parseAnd()`, the function will simply return the result of `parseAnd()`.
- **Mixed Precedence**: The function correctly handles mixed precedence between logical OR and logical AND due to the recursive nature of calling `parseAnd()` inside the loop.
- **Trailing Whitespace**: Any trailing whitespace after an operator is handled gracefully by the `skipNewlines()` function.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens (`current()`, `consume()`). These tokens represent the operators and operands in the source code.
- **Error Handling**: While not shown in the snippet, the function likely integrates with error handling mechanisms to manage unexpected tokens or syntax errors.
- **Abstract Syntax Tree Construction**: The function directly contributes to building the AST by creating nodes for binary expressions. Other parts of the compiler may use this AST for further semantic analysis, code generation, or optimization.

Overall, the `parseOr` function efficiently parses complex expressions involving multiple logical operators, ensuring correct precedence and structure in the resulting AST.