# `parseAddSub`

## Function Overview

The `parseAddSub` function is a crucial method within the Quantum Language compiler's parser, responsible for parsing arithmetic expressions that incorporate both addition (`+`) and subtraction (`-`). This function builds upon the lower-level parsing capabilities provided by `parseMulDiv`, which handles multiplication and division operations.

### Why It Works This Way

The design of `parseAddSub` allows for the recursive parsing of expressions involving multiple levels of addition and subtraction. By repeatedly calling `parseMulDiv` and consuming tokens representing the operators (`+` or `-`), the function constructs a binary expression tree where each node represents an operation and its operands. This approach ensures that the correct order of operations is respected, as specified by the language grammar rules.

## Parameters and Return Value

### Parameters

- None

### Return Value

- `std::unique_ptr<ASTNode>`: A unique pointer to the root of the abstract syntax tree (AST) representing the parsed arithmetic expression. The AST is built using nodes of type `BinaryExpr`, which encapsulate the operator and its two operands.

## Edge Cases

1. **Single Operand**: If the input expression consists of a single operand, such as `5`, `parseAddSub` will return an AST node representing that operand without any additional operations.
2. **No Operators**: If there are no addition or subtraction operators in the expression, `parseAddSub` will simply call `parseMulDiv` once and return its result.
3. **Nested Expressions**: The function can handle nested expressions, such as `(3 + 4) - 5`. It will correctly parse the innermost expression first and then combine it with the outer expression according to the operator precedence.

## Interactions with Other Components

- **Tokenizer**: `parseAddSub` relies on the tokenizer to provide tokens representing numbers, operators, and parentheses. These tokens are consumed by the parser to construct the AST.
- **Error Handling**: During the parsing process, `parseAddSub` checks for valid token sequences and reports errors if unexpected tokens are encountered. This helps maintain the integrity of the parsed expressions and provides feedback for debugging.
- **Operator Precedence**: The function respects the operator precedence rules of the language, ensuring that operations are evaluated in the correct order. For example, multiplication and division are performed before addition and subtraction.

By leveraging these interactions, `parseAddSub` effectively parses complex arithmetic expressions into a structured representation, facilitating further analysis and code generation within the compiler.