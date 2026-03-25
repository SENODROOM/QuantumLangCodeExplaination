# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`). This function ensures that the syntax tree accurately reflects the structure and semantics of the parsed comparison expressions.

## Parameters/Return Value
- **Parameters**: None explicitly defined within the function signature; it operates on global state managed by the parser.
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed comparison expression. If the parsing fails at any point, it throws a `ParseError`.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseShift()` to parse the left-hand side (LHS) of the comparison expression. The result is stored in the variable `left`.
2. **Loop for Comparisons**: The function enters a loop that continues as long as the next token is one of the comparison or membership operators (`LT`, `GT`, `LTE`, `GTE`, `IN`, `NOT`, `IS`). Inside the loop:
   - It records the current line number (`ln`) where the comparison begins.
   - Depending on the type of operator encountered:
     - For `IS` and `IS NOT`: It checks if the subsequent token is `NOT`. If so, it parses the right-hand side (`RHS`) using `parseShift()`, constructs a `BinaryExpr` node with the appropriate operator, and updates `left` accordingly.
     - For `NOT IN`: It consumes the `NOT` token, verifies that the next token is `IN`, then parses the RHS and constructs a `BinaryExpr` node with "not in".
     - For other comparison operators (`<`, `>`, `<=`, `>=`): It consumes the operator token, parses the RHS, and constructs a `BinaryExpr` node with the consumed operator.
3. **Edge Cases**:
   - If the expected token after `NOT` is not `IN`, the function throws a `ParseError`.
   - The function assumes that `parseShift()` correctly handles parsing of individual terms and shifts through the tokens appropriately.
4. **Interactions with Other Components**:
   - This function interacts with the global state of the parser, accessing the current token (`current()`) and advancing the token stream (`consume()`).
   - It utilizes helper functions like `check()` and `match()` to determine the presence and consumption of specific tokens.
   - The constructed `ASTNode` objects represent nodes in the abstract syntax tree (AST) and are used by other parts of the compiler to perform semantic analysis and code generation.

## Example Usage
Here's a simplified example of how `parseComparison` might be invoked in the context of parsing a quantum program:

```cpp
auto rootNode = parseProgram();
// Assume rootNode contains the AST for the entire program
```

In this example, `parseProgram()` would internally call `parseComparison()` multiple times to construct the AST for all comparison expressions found in the quantum program.

This comprehensive approach ensures that complex comparison expressions are accurately parsed and represented in the AST, facilitating further processing by other stages of the compiler.