# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an essential component of the Quantum Language compiler's parser, responsible for interpreting expressions that involve multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures proper parsing and construction of abstract syntax trees (ASTs) for these operations.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but it implicitly uses global state managed by the parser, such as the current token and methods to advance through tokens.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode`, representing the parsed expression involving multiplication, division, modulo, or floor division.

## How It Works

1. **Initial Parsing**:
   - The function starts by calling `parsePower()`, which parses expressions involving exponentiation (`**`) and unary operators. The result of this call is stored in the variable `left`.

2. **Loop for Multiplicative Operations**:
   - The function enters a loop that continues as long as the current token is one of the following types: `TokenType::STAR` (multiplication), `TokenType::SLASH` (division), `TokenType::PERCENT` (modulo), or `TokenType::FLOOR_DIV` (floor division).
   
3. **Token Consumption and AST Construction**:
   - Inside the loop, the function consumes the current token using `consume()` and retrieves its value (the operator). The line number of the current token is also recorded.
   - The function then calls `parsePower()` again to parse the next part of the expression, storing the result in the variable `right`.
   - A new `ASTNode` is created with a `BinaryExpr` containing the operator, the previously parsed `left` expression, and the newly parsed `right` expression. This node is wrapped in a unique pointer and assigned back to `left`.

4. **Edge Cases**:
   - If there are no more multiplicative or division operators after the initial call to `parsePower()`, the loop will not execute, and the function will simply return the `left` node.
   - If any of the tokens consumed during the loop are not recognized as valid operators, the behavior is undefined and may lead to errors in parsing.

5. **Interactions with Other Components**:
   - The `parseMulDiv` function interacts with the `Tokenizer` to retrieve the current token and advance through the input stream.
   - It also utilizes the `ErrorReporter` to report any syntax errors encountered during parsing.
   - The `SymbolTable` might be used to resolve identifiers within the expressions, although this is not directly shown in the provided code snippet.

By recursively parsing sub-expressions and constructing binary expressions based on the detected operators, the `parseMulDiv` function effectively handles complex arithmetic expressions in the Quantum Language, ensuring they are correctly represented in the AST for further compilation stages.