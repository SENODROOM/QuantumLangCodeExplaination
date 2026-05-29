# `parsePower`

## Description

The `parsePower` function is a crucial component of the parser within the Quantum Language compiler. Its primary role is to interpret and construct Abstract Syntax Tree (AST) nodes representing expressions involving the power operator (`**`). This function facilitates the parsing process by recursively handling nested power operations, ensuring they are evaluated correctly according to their right-associativity.

## Parameters/Return Value

### Parameters
- None

### Return Value
- Returns a unique pointer to an `ASTNode` object representing the parsed power expression. If no power operation is found, it returns the result of `parseUnary()`.

## Edge Cases

1. **No Power Operator**: If the input stream does not contain any power operators (`**`), the function will simply return the result of `parseUnary()`, which could be a number, variable, or another unary expression.
2. **Nested Powers**: The function handles nested power operations correctly due to its recursive nature. For example, the expression `a ** b ** c` would be interpreted as `a ** (b ** c)`.

## Interactions with Other Components

- **parseUnary()**: This function is called at the beginning of `parsePower()`. It parses expressions that can occur before a power operator, such as numbers, variables, or parentheses enclosing an expression.
  
- **check(TokenType::POWER)**: This method checks whether the current token in the input stream is a power operator (`**`). If it is, the function proceeds to parse the right-hand side of the power expression.

- **consume()**: After identifying a power operator, this method consumes the current token, advancing the input stream to the next token for further parsing.

- **std::make_unique<ASTNode>()**: This utility function creates a new `ASTNode` object with a `BinaryExpr` type, setting the operator to `"**"`, and moving the parsed left and right operands into the node. The line number (`ln`) is also stored to maintain context in the source code.

## Why It Works This Way

The design of `parsePower` follows a top-down approach to parsing expressions, where each function is responsible for parsing a specific type of expression. By calling `parseUnary()` first, we ensure that any valid unary expression can be parsed before encountering a power operator. 

The use of recursion allows `parsePower` to handle nested power operations naturally. When a power operator is encountered, the function calls itself again to parse the right-hand side of the expression, effectively creating a tree structure where powers are applied from right to left.

This approach ensures that the parser correctly interprets complex expressions involving multiple power operations, adhering to the rules of operator precedence and associativity in mathematics.

## Example Usage

Here's how you might use `parsePower()` in the context of parsing a quantum language expression:

```cpp
auto expr = parsePower();
if (expr)
{
    // Process the AST node representing the power expression
    // ...
}
else
{
    // Handle the case where no valid expression was parsed
    // ...
}
```

In this example, `parsePower()` is invoked to parse a quantum language expression. If a valid expression is successfully parsed, it is returned as a unique pointer to an `ASTNode`. Otherwise, appropriate error handling is performed.