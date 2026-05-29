# `parseAddSub`

## Function Overview

The `parseAddSub` function is an essential component of the Quantum Language compiler's parser, designed to handle the parsing of arithmetic expressions that include both addition (`+`) and subtraction (`-`). This function leverages the capabilities provided by its predecessor, `parseMulDiv`, which is responsible for parsing multiplication and division operations.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed arithmetic expression involving addition and/or subtraction.

### How It Works

1. **Initial Parsing**: The function begins by calling `parseMulDiv()` to obtain the initial term or sub-expression in the arithmetic expression. This term is stored in the variable `left`.

2. **Loop for Addition/Subtraction Operations**:
   - The function enters a loop that continues as long as the next token is either a plus sign (`TokenType::PLUS`) or a minus sign (`TokenType::MINUS`).
   - Inside the loop, it records the current line number using `current().line`.
   - The operator (`op`) is consumed from the input stream using `consume()`. The value of this operator is stored in the variable `op`.
   - Another call to `parseMulDiv()` is made to parse the next term or sub-expression, which becomes the `right` operand of the current operation.

3. **Building the AST**:
   - The parsed terms (`left` and `right`) along with the operator (`op`) are combined into a new `BinaryExpr` node.
   - This `BinaryExpr` node is then wrapped inside an `ASTNode` and assigned back to the `left` variable. This process effectively chains together multiple addition and subtraction operations into a single abstract syntax tree (AST).

4. **Edge Cases**:
   - If there are no more tokens after the initial call to `parseMulDiv()`, the function simply returns the `left` term without entering the loop.
   - The loop ensures that all consecutive addition and subtraction operations are correctly parsed and chained together, regardless of their order in the input expression.

5. **Interactions with Other Components**:
   - `parseAddSub` relies on `parseMulDiv` to break down the expression into smaller parts, handling multiplication and division first before moving on to addition and subtraction.
   - The function also interacts with the lexer through methods like `current()` and `consume()` to retrieve and advance the token stream, respectively.
   - The resulting AST can be used by subsequent phases of the compiler, such as semantic analysis and code generation, to understand and process the arithmetic expression accurately.

In summary, the `parseAddSub` function plays a pivotal role in parsing arithmetic expressions involving addition and subtraction by chaining them together into a single AST. Its design ensures correct handling of nested operations and provides a solid foundation for further processing in the compiler.