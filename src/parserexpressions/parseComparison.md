# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`).

## Parameters
- None

## Return Value
- `std::unique_ptr<ASTNode>`: A pointer to an abstract syntax tree node representing the parsed comparison expression.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseShift()` to parse the left-hand side (LHS) of the comparison expression. This LHS is stored in the variable `left`.

2. **Loop for Comparisons**: The function then enters a loop that continues as long as the next token is one of the following comparison operators:
   - `<`
   - `>`
   - `<=`
   - `>=`
   - `in`
   - `is`
   - `not`
   - `is not`

3. **Handling Operators**:
   - If the current token is `is` or `is not`, the function checks for the presence of `not`. If `not` is found, it parses the right-hand side (RHS) using `parseShift()`, creates a new `BinaryExpr` node with the operator "not in" or "is not", and updates `left`.
   - If the current token is `in`, the function consumes it and parses the RHS using `parseShift()`, creating a new `BinaryExpr` node with the operator "in" and updating `left`.
   - For all other comparison operators (`<`, `>`, `<=`, `>=`), the function consumes the operator, parses the RHS using `parseShift()`, and creates a new `BinaryExpr` node with the consumed operator and the parsed LHS and RHS, updating `left`.

4. **Edge Cases**:
   - If the token sequence is invalid (e.g., expecting `in` after `not` but finding something else), the function throws a `ParseError`.
   - The function ensures proper handling of nested comparisons and correctly constructs the AST nodes for each comparison.

5. **Interactions with Other Components**:
   - The function relies on `parseShift()` to handle the parsing of individual terms within the comparison expression.
   - It uses `consume()` to advance the tokenizer to the next token after processing the current comparison operator.
   - The function interacts with the tokenizer through methods like `check()`, which verifies if the current token matches a specified type, and `match()`, which consumes the current token if it matches the specified type.
   - The AST nodes constructed by `parseComparison` are used by subsequent stages of the compiler to perform semantic analysis and code generation.

This function is crucial for accurately parsing and constructing comparison expressions in the Quantum Language, ensuring that the resulting AST can be effectively analyzed and utilized by later parts of the compiler.