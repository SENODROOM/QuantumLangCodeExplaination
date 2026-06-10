# `parseAddSub`

## Function Overview

The `parseAddSub` function is a crucial component of the Quantum Language compiler's parser, designed to handle the parsing of arithmetic expressions that include both addition (`+`) and subtraction (`-`). This function operates by recursively calling itself to parse multiplication and division expressions, then combining them using binary operations based on the encountered tokens.

## Parameters/Return Value

### Parameters
- None

### Return Value
- `std::unique_ptr<ASTNode>`: A unique pointer to an abstract syntax tree node representing the parsed arithmetic expression.

## Why It Works This Way

The `parseAddSub` function works in a bottom-up manner, starting with the most basic arithmetic operations (multiplication and division) handled by the `parseMulDiv()` function. As these operations are parsed, they form the initial nodes of the AST. The function then checks for subsequent addition or subtraction operators using the `check()` method. If such an operator is found, it consumes the token using the `consume()` method and recursively calls `parseMulDiv()` again to parse the next set of operations. These results are combined into a new binary operation node, which replaces the previously parsed node as the new "left" operand. This process continues until no more addition or subtraction operators are found.

This approach ensures that the arithmetic operations are evaluated according to their precedence rules, with multiplication and division being performed before addition and subtraction. By using recursive descent parsing, the function can easily handle nested expressions and maintain a clear stack of operations.

## Edge Cases

1. **Empty Expression**: If there are no tokens to parse, the function will simply return `nullptr`, indicating an empty expression.
2. **Single Token**: If only one token is present, it will be returned directly as a single-node AST.
3. **Mixed Operations**: The function correctly handles expressions with mixed addition and subtraction operations, evaluating them from left to right.
4. **Whitespace and Comments**: The function should ignore any whitespace and comments between tokens, ensuring accurate parsing.

## Interactions With Other Components

- **Tokenizer**: The `parseAddSub` function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function may interact with error handling mechanisms within the compiler to report issues such as missing operands or invalid tokens.
- **AST Construction**: The function constructs an abstract syntax tree (AST) by creating `ASTNode` instances representing different types of expressions and operations. This AST is used for further semantic analysis and code generation phases of the compilation process.

In summary, the `parseAddSub` function plays a vital role in parsing arithmetic expressions involving addition and subtraction in the Quantum Language compiler. Its design allows for efficient handling of complex expressions through recursive descent parsing, ensuring correct evaluation of operations based on their precedence rules.