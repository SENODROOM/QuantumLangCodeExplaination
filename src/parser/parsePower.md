# `parsePower` Function

## Purpose
The `parsePower` function is designed to parse expressions involving the power operator (`**`). It recursively processes the expression on its left and then consumes the power token before parsing the expression on its right. This ensures that the power operation is evaluated right-associatively, meaning that expressions like `a ** b ** c` will be interpreted as `a ** (b ** c)` rather than `(a ** b) ** c`.

## Parameters
- None

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed power expression. If there is no power operator in the current expression, it returns the result of `parseUnary()`.

## Edge Cases
1. **No Power Operator**: If the current token is not a power operator (`TokenType::POWER`), the function simply returns the result of `parseUnary()`. This handles cases where the expression does not involve any power operations.
2. **Nested Power Operations**: The function supports nested power operations, such as `a ** b ** c`, which are correctly parsed as `a ** (b ** c)` due to its right-associative nature.
3. **Invalid Tokens**: If the current token after consuming the power operator is invalid or unexpected, the function may throw an exception or handle it appropriately based on the error handling mechanism defined in the compiler.

## Interactions with Other Components
- **Parsing Unary Expressions**: The function calls `parseUnary()` to get the left-hand side of the power expression. `parseUnary()` is responsible for parsing expressions that can appear before the unary operators (`+`, `-`, `!`, `~`) and literals.
- **Token Consumption**: After identifying the power operator, the function consumes it using the `consume()` method. This moves the parser's current token forward to the next token in the input stream.
- **Recursive Parsing**: The function recursively calls itself to parse the right-hand side of the power expression. This allows it to handle complex expressions involving multiple levels of power operations.
- **Error Handling**: The function interacts with the overall error handling mechanism of the compiler. If an error occurs during the parsing process, such as encountering an unexpected token, the function may report an error or terminate the compilation process gracefully.

## Implementation Details
Here is the implementation of the `parsePower` function:

```cpp
{
    auto left = parseUnary();  // Parse the left-hand side of the power expression
    if (check(TokenType::POWER))  // Check if the current token is a power operator
    {
        int ln = current().line;  // Get the line number of the current token
        consume();  // Consume the power operator token
        auto right = parsePower();  // Recursively parse the right-hand side of the power expression
        return std::make_unique<ASTNode>(BinaryExpr{"**", std::move(left), std::move(right)}, ln);  // Create an ASTNode for the power expression
    }
    return left;  // Return the parsed left-hand side if no power operator is found
}
```

### Explanation of Key Steps
1. **Parse Left-Hand Side**: The function starts by calling `parseUnary()` to parse the left-hand side of the power expression. This step ensures that any valid unary expression can be processed.
2. **Check for Power Operator**: It then checks if the current token is a power operator (`TokenType::POWER`) using the `check()` method. If the check passes, it means there is a power operation to be parsed.
3. **Consume Power Operator**: If a power operator is found, the function consumes it using the `consume()` method. This updates the parser's state to move to the next token.
4. **Recursively Parse Right-Hand Side**: The function recursively calls itself (`parsePower()`) to parse the right-hand side of the power expression. This recursion continues until no more power operators are found.
5. **Create ASTNode**: Once both sides of the power expression have been parsed, the function creates an `ASTNode` representing the binary expression with the operator `"**"`. The `std::move()` is used to transfer ownership of the parsed left and right nodes into the new `ASTNode`.
6. **Return Result**: Finally, the function returns the newly created `ASTNode`. If no power operator was found initially, it simply returns the result of `parseUnary()`.

This implementation ensures that the power operator is handled correctly in the context of larger expressions, maintaining the correct order of operations through recursive parsing.