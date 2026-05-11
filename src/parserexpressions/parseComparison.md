# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`). This function ensures that the syntax tree accurately reflects the structure of the comparison expression being parsed.

## Parameters
- **None**: The function operates on the global state of the parser, which includes the current token and the ability to consume tokens.

## Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an `ASTNode` representing the parsed comparison expression. If the parsing fails, an exception is thrown.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseShift()` to parse the left-hand side (LHS) of the comparison expression. The result is stored in the `left` variable.

2. **Loop for Comparisons**: The function enters a loop that continues as long as the next token is one of the following comparison operators:
   - `<` (less than)
   - `>` (greater than)
   - `<=` (less than or equal to)
   - `>=` (greater than or equal to)
   - `in` (membership test)
   - `is` (identity test)
   - `not` (negation)

3. **Handling Comparison Operators**:
   - For `is` and `is not` operators:
     - The function consumes the `is` or `is not` token.
     - If `is not` is encountered, it checks the next token for `not`. If found, it parses the right-hand side (RHS) using `parseShift()`.
     - A new `BinaryExpr` node is created with the operator and the LHS and RHS nodes, and the line number of the current token.
   - For `in` and `not in` operators:
     - The function consumes the `in` or `not in` token.
     - If `not in` is encountered, it checks the next token for `not`. If not found, it throws a `ParseError`.
     - The RHS is then parsed using `parseShift()`.
     - A new `BinaryExpr` node is created with the operator and the LHS and RHS nodes, and the line number of the current token.
   - For other comparison operators (`<`, `>`, `<=`, `>=`):
     - The function consumes the comparison operator token.
     - The RHS is parsed using `parseShift()`.
     - A new `BinaryExpr` node is created with the operator and the LHS and RHS nodes, and the line number of the current token.

4. **Return Statement**: After the loop completes, the function returns the `left` variable, which now contains the fully parsed comparison expression.

## Edge Cases
- **Invalid Negations**: If `not in` is encountered without `not` immediately following it, the function throws a `ParseError`.
- **Empty Expression**: If there are no comparison operators after the initial LHS parsing, the function simply returns the LHS node.
- **Syntax Errors**: If any unexpected token is encountered during parsing, the function will throw a `ParseError`.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **ASTNode**: The function constructs an abstract syntax tree (AST) using `ASTNode` objects, which represent different parts of the parsed expression.
- **BinaryExpr**: Within the AST construction, the function uses `BinaryExpr` objects to encapsulate binary operations like comparison.
- **Error Handling**: The function incorporates error handling mechanisms provided by the parser's error reporting system to manage syntax errors gracefully.

This function is crucial for correctly interpreting and constructing comparison expressions within the Quantum Language compiler, ensuring that the resulting AST accurately represents the intended logic of the code.