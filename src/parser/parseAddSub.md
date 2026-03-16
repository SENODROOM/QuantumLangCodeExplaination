# parseAddSub

The `parseAddSub` function in the Quantum Language compiler's `Parser.cpp` file is designed to handle the parsing of expressions that involve addition (`+`) and subtraction (`-`). This function plays a vital role in constructing an Abstract Syntax Tree (AST) that accurately reflects the mathematical structure of these expressions. 

## What It Does

`parseAddSub` begins by calling another function named `parseMulDiv()`, which is responsible for parsing multiplication, division, and unary operations. The result of `parseMulDiv()` is stored as the initial left-hand side (`left`) of the expression being parsed. 

Next, the function enters a loop that continues as long as the next token is either a `PLUS` or a `MINUS`. Inside the loop:
- The line number (`ln`) of the current token is recorded.
- The operator (`op`) is consumed and its value is retrieved.
- Another call to `parseMulDiv()` is made to parse the right-hand side (`right`) of the current operation.
- A new `ASTNode` is created using the `BinaryExpr` constructor, which encapsulates the operator and the two operands (`left` and `right`). This new node becomes the new `left` for the next iteration of the loop, effectively building up the AST level by level.

This process repeats until there are no more `PLUS` or `MINUS` operators to process, at which point the function returns the final `ASTNode`.

## Why It Works This Way

The design of `parseAddSub` follows a recursive descent approach, where each function is responsible for parsing a specific part of the grammar. By breaking down the problem into smaller parts—first parsing multiplicative/divisive expressions and then handling addition/subtraction—the function can construct a nested AST that correctly represents the precedence and associativity rules of arithmetic operations.

Using a loop to repeatedly parse subsequent terms ensures that all operations are processed in the order they appear in the input expression. This is crucial because addition and subtraction have lower precedence than multiplication and division, and they are also left associative. Therefore, expressions like `a + b - c` should be interpreted as `(a + b) - c`, not `a + (b - c)`.

## Parameters/Return Value

### Parameters

- None directly specified in the provided code snippet. However, based on typical parser implementations, `parseAddSub()` likely relies on global state or context variables such as:
  - `currentToken`: The current token being processed.
  - `tokens`: A list or stream of tokens representing the input expression.
  - `consume()`: A method that advances the `currentToken` pointer to the next token and returns the previous token.

### Return Value

- Returns a `std::unique_ptr<ASTNode>` representing the root of the AST constructed for the parsed expression. This node contains the entire expression tree, including all addition and subtraction operations along with their associated operands.

## Edge Cases

1. **Empty Expression**: If the input expression is empty or consists only of whitespace, `parseMulDiv()` might return a null pointer. In this case, `parseAddSub()` would return a null pointer as well.
   
2. **Single Term**: If the input expression contains only one term (e.g., `5`), `parseMulDiv()` will return an AST node for that term. Since there are no `PLUS` or `MINUS` operators to process, the function will simply return this single-term node.

3. **Mixed Operations**: The function correctly handles mixed operations by respecting the precedence and associativity rules. For example, `a * b + c / d` will be parsed as `((a * b) + (c / d))`.

4. **Whitespace**: Whitespace between tokens is ignored, allowing for expressions like `a +   b - c`.

## Interactions With Other Components

- **parseMulDiv()**: This function is called twice within `parseAddSub()`. Once to get the initial left-hand side, and once for each subsequent term. The results from these calls are used to build the AST.

- **AST Construction**: The `ASTNode` class is used to create nodes in the AST. Each node represents either an operand or an operation. The `BinaryExpr` constructor specifically is used to create nodes for binary operations (addition and subtraction).

- **Error Handling**: While not shown in the snippet, typical parsers include error handling mechanisms. These might involve checking for unexpected tokens or malformed expressions and reporting errors accordingly.

Overall, `parseAddSub` is a fundamental component of the Quantum Language compiler's parser, ensuring that expressions involving basic arithmetic operations are correctly parsed and represented in the AST.