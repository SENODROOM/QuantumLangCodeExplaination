# `parseAddSub`

## Function Overview

The `parseAddSub` function is an essential component of the Quantum Language compiler's parser, designed to handle the parsing of arithmetic expressions involving both addition (`+`) and subtraction (`-`). This function leverages the recursive descent parsing technique to construct an Abstract Syntax Tree (AST) representing the parsed expression.

### Parameters

This function takes no explicit parameters.

### Return Value

The function returns a unique pointer to an `ASTNode`, which represents the root of the AST constructed for the arithmetic expression.

### Edge Cases

1. **Empty Expression**: If there is no valid token sequence to form an arithmetic expression, the function should gracefully handle this case without throwing errors.
2. **Single Term**: If the input expression consists of only one term (e.g., `5` or `x`), the function should correctly parse and return it as a single node in the AST.
3. **Nested Expressions**: The function should be able to handle nested expressions properly, constructing the AST according to the operator precedence rules.

### Interactions with Other Components

The `parseAddSub` function interacts closely with the following components:

1. **Tokenizer**: It consumes tokens produced by the tokenizer to build the AST.
2. **parseMulDiv**: This helper function is called within `parseAddSub` to parse multiplication and division operations. Since addition and subtraction have higher precedence than multiplication and division, `parseAddSub` calls `parseMulDiv` first to ensure correct parsing order.
3. **ASTNode**: The function constructs an `ASTNode` using the parsed tokens and recursively built subtrees. Each `ASTNode` can represent either a terminal (a number or variable) or a non-terminal (an operation like `+` or `-`).

### Detailed Explanation

The `parseAddSub` function begins by calling the `parseMulDiv` function to parse the initial term of the arithmetic expression. This term could be a number, a variable, or a more complex expression enclosed in parentheses.

Once the initial term is parsed, the function enters a loop that continues as long as the next token is either a `PLUS` or `MINUS` token. Inside the loop:
- The current line number is recorded using `current().line`.
- The operator (`+` or `-`) is consumed using `consume()`. The operator's value is stored in the `op` variable.
- Another call to `parseMulDiv()` is made to parse the next term of the expression.
- An `ASTNode` is created with a `BinaryExpr` containing the operator and the two parsed terms. The new node becomes the left child of the previous node, and the right child is set to the result of the second call to `parseMulDiv()`.
- The line number is passed to the `ASTNode` constructor to maintain context information.

This process repeats until all terms in the expression have been parsed and combined into a binary tree structure, where each node represents an operation and its operands.

Finally, the function returns the unique pointer to the root `ASTNode` of the constructed expression tree.

By utilizing this approach, the `parseAddSub` function ensures that arithmetic expressions are parsed correctly according to their operator precedence rules, allowing for the construction of accurate and meaningful ASTs that can be further processed by the compiler.