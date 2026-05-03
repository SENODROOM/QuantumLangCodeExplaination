# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an integral part of the Quantum Language compiler's parser, tasked with interpreting expressions that include multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures that these arithmetic operations are correctly parsed and structured within the abstract syntax tree (AST).

## Parameters and Return Value

- **Parameters**: None explicitly listed in the provided code snippet.
- **Return Value**: The function returns a unique pointer to an `ASTNode`, which represents the root of a subtree corresponding to the parsed expression involving multiplication, division, modulo, or floor division.

## How It Works

1. **Initial Parsing**: The function begins by calling `parsePower()`, which parses the left-hand side of the expression. This initial call sets up the `left` variable with the result of the power operation.

2. **Loop for Multiplicative Operations**: The function then enters a loop that continues as long as the next token is one of the specified multiplicative operators (`*`, `/`, `%`, `//`). Inside the loop:
   - **Line Number Retrieval**: The line number of the current token is retrieved using `current().line`.
   - **Operator Consumption**: The operator token is consumed using `consume()`, and its value is stored in the `op` variable.
   - **Right Hand Side Parsing**: Another call to `parsePower()` is made to parse the right-hand side of the expression, setting the `right` variable.
   - **Tree Construction**: A new `ASTNode` is constructed using the `BinaryExpr` structure, encapsulating the operator and the two operands (`left` and `right`). This node is then assigned back to `left`, effectively building the AST from bottom to top.

3. **Termination**: Once there are no more multiplicative operators following the current token, the loop terminates, and the function returns the final `left` node, which now represents the entire parsed expression.

## Edge Cases

- **No Operators**: If the expression consists solely of a single term without any of the specified operators, the function will simply return the result of `parsePower()`.
- **Nested Expressions**: The function handles nested expressions gracefully by recursively calling itself when encountering another `parseMulDiv()` construct within the parsed terms.
- **Invalid Tokens**: If the next token after `parsePower()` is not one of the expected multiplicative operators, the loop condition fails, and the function returns the initial `left` node.

## Interactions with Other Components

- **Tokenizer**: The `parseMulDiv` function relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()` and `consume()` to interact with the tokenizer.
- **Abstract Syntax Tree (AST)**: The function constructs nodes in the AST based on the parsed expressions. These nodes can be further processed by subsequent stages of the compiler, such as semantic analysis and code generation.
- **Error Handling**: While not shown in the snippet, the function should integrate with the error handling mechanisms of the compiler to manage unexpected tokens or malformed expressions.

By ensuring that all multiplicative operations are correctly parsed and integrated into the AST, the `parseMulDiv` function facilitates the compilation process, allowing for accurate representation and manipulation of quantum language expressions.