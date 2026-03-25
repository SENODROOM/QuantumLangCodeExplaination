# `parsePower`

## Description

The `parsePower` function is part of the parser in the Quantum Language compiler and is responsible for parsing expressions involving the power operator (`**`). This function follows a recursive descent approach to handle nested power operations, ensuring that they are evaluated correctly due to their right-associativity.

## Parameters

This function takes no explicit parameters. It relies on global state managed by the parser, such as the current token being processed and methods like `check`, `consume`, and `parseUnary`.

## Return Value

The function returns a unique pointer to an `ASTNode` representing the parsed expression. If the current token is not a power operator (`TokenType::POWER`), it simply returns the result of `parseUnary`.

## Edge Cases

1. **No Power Operator**: If there is no power operator after the initial unary expression, the function will return the unary expression itself without any modification.
2. **Nested Powers**: The function handles nested power expressions correctly due to its recursive nature. For example, the expression `a ** b ** c` will be parsed as `a ** (b ** c)`.
3. **Single Token Expressions**: If the input consists of a single token, which could be a number or a variable, the function will return an AST node representing that token.

## Interactions with Other Components

- **parseUnary**: This function is called at the beginning of `parsePower`. It parses an initial unary expression (e.g., `-a`, `+b`, `!c`) and returns it as an `ASTNode`.
- **check**: This method checks whether the current token matches the specified `TokenType::POWER`. It is used to determine if the next token should be treated as a power operator.
- **consume**: This method advances the parser to the next token. It is called when a match is found, indicating that the current token has been consumed.
- **BinaryExpr**: When a power operator is encountered, a new `BinaryExpr` object is created with the operator `"**"`, the left-hand side as the result of `parseUnary`, and the right-hand side as the result of another call to `parsePower`. This ensures that powers are evaluated from right to left.

## Code Explanation

Here's a breakdown of the code:

```cpp
auto left = parseUnary();
if (check(TokenType::POWER)) {
    int ln = current().line; // Store the line number where the power operation starts
    consume(); // Move to the next token (the power operator)
    auto right = parsePower(); // Recursively parse the right-hand side of the power operation
    return std::make_unique<ASTNode>(BinaryExpr{"**", std::move(left), std::move(right)}, ln); // Create and return the ASTNode
}
return left; // If no power operator is found, return the initial unary expression
```

### Detailed Breakdown

1. **Initial Unary Expression**:
   ```cpp
   auto left = parseUnary();
   ```
   - This line calls `parseUnary()` to parse the initial unary expression and stores the result in the `left` variable.

2. **Check for Power Operator**:
   ```cpp
   if (check(TokenType::POWER)) {
   ```
   - The `check` method is used to verify if the current token is a power operator (`TokenType::POWER`). If it is, the function proceeds to parse the power expression.

3. **Store Line Number**:
   ```cpp
   int ln = current().line;
   ```
   - The line number where the power operation starts is stored in the `ln` variable. This information is crucial for error reporting and debugging.

4. **Consume Power Operator**:
   ```cpp
   consume();
   ```
   - The `consume` method moves the parser to the next token, effectively skipping over the power operator.

5. **Recursive Parsing of Right-Hand Side**:
   ```cpp
   auto right = parsePower();
   ```
   - Another call to `parsePower()` is made recursively to parse the right-hand side of the power operation. This ensures that the rightmost power operation is evaluated first, adhering to the right-associativity rule.

6. **Create and Return ASTNode**:
   ```cpp
   return std::make_unique<ASTNode>(BinaryExpr{"**", std::move(left), std::move(right)}, ln);
   ```
   - A new `BinaryExpr` object is created with the operator `"**"`, the left-hand side (`left`), and the right-hand side (`right`). The `std::move` keyword is used to transfer ownership of the `left` and `right` nodes to the `BinaryExpr`, preventing unnecessary copies.
   - An `ASTNode` containing the `BinaryExpr` is returned. The line number `ln` is passed to ensure accurate source location tracking in the abstract syntax tree.

7. **Return Initial Unary Expression**:
   ```cpp
   return left;
   ```
   - If no power operator is found, the function directly returns the initial unary expression represented by `left`.

## Conclusion

The `parsePower` function plays a critical role in parsing expressions involving the power operator in the Quantum Language compiler. By leveraging recursion and handling right-associativity, it ensures that complex power expressions are parsed accurately and efficiently. Its interaction with other parser functions like `parseUnary`, `check`, and `consume` highlights its importance in building a robust and flexible parser capable of handling various quantum language constructs.