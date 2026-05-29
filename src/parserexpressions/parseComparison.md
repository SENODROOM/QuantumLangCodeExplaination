# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`).

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed comparison expression.

## How It Works
The function starts by parsing a shift expression using the `parseShift()` method. It then enters a loop that continues as long as the next token is one of the comparison or membership operators (`<`, `>`, `<=`, `>=`, `in`, `is`, `not`). Inside the loop:
1. If the current token is `is`, the function checks for the presence of `not`. If found, it parses another shift expression and constructs a `BinaryExpr` node with the "is not" operator. Otherwise, it parses another shift expression and constructs a `BinaryExpr` node with the "is" operator.
2. If the current token is `not`, the function consumes it and checks for the presence of `in`. If found, it parses another shift expression and constructs a `BinaryExpr` node with the "not in" operator. If not found, it throws a `ParseError`.
3. For any other comparison operator (`<`, `>`, `<=`, `>=`), the function consumes the operator, parses another shift expression, and constructs a `BinaryExpr` node with the consumed operator.
4. The constructed `BinaryExpr` nodes are wrapped in `ASTNode` objects, which include the line number of the original tokens for error reporting purposes.

## Edge Cases
- **Empty Input**: If there are no tokens left to parse, the function will return the initial shift expression without constructing any additional `BinaryExpr` nodes.
- **Invalid Negation**: If `not` is followed by a token other than `in` or `is`, the function will throw a `ParseError`.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **ASTNode**: The function uses `ASTNode` to construct the abstract syntax tree (AST) representation of the parsed comparison expressions.
- **BinaryExpr**: The function creates instances of `BinaryExpr` to represent binary operations in the AST, such as comparisons and membership tests.

This function ensures that all valid comparison and membership expressions are correctly parsed and represented in the AST, facilitating further processing by subsequent stages of the compiler.