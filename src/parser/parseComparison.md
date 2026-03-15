# `parseComparison` Function

## Overview

The `parseComparison` function is part of the Quantum Language compiler's parser and is responsible for parsing comparison expressions. These expressions include relational operators (`<`, `>`, `<=`, `>=`) as well as membership tests (`in`, `is`, `is not`). The function constructs an Abstract Syntax Tree (AST) node representing the parsed comparison expression.

## Parameters

- None explicitly declared; however, it relies on global state managed by the parser, such as the current token being processed.

## Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed comparison expression.

## How It Works

1. **Initial Parsing**: The function starts by calling `parseShift()` to parse the left-hand side (LHS) of the comparison expression. This LHS can be any valid expression that the `parseShift` method can handle.

2. **Loop for Comparison Operators**: The function enters a loop that continues as long as the next token matches one of the comparison or membership test operators (`<`, `>`, `<=`, `>=`, `in`, `is`, `is not`). For each matching operator:
   - It consumes the current token.
   - Depending on the operator type, it further processes the tokens:
     - If the operator is `is` or `is not`, it checks for a subsequent `not` keyword. If found, it parses the right-hand side (RHS) using `parseShift()`, creates a `BinaryExpr` node with the appropriate operator, and updates the `left` variable.
     - If the operator is `in` or `not in`, it directly parses the RHS using `parseShift()`, creates a `BinaryExpr` node with the operator, and updates the `left` variable.
     - For all other relational operators, it simply parses the RHS and creates a `BinaryExpr` node with the operator.

3. **Edge Cases**:
   - The function handles the special case of `is not` by checking for the `not` keyword immediately following `is`. If `not` is found, it constructs a `BinaryExpr` with "is not"; otherwise, it constructs a `BinaryExpr` with "is".
   - The function ensures that "not in" is treated as a single two-token operator by checking for both "not" and "in" sequentially. If "not" is followed by something other than "in", it throws a `ParseError`.

4. **Interactions with Other Components**:
   - The function interacts with the `consume()` method to advance the token stream and retrieve the current token.
   - It uses the `check()` method to determine if the next token matches a particular type of operator.
   - The `match()` method is used to consume a token if it matches a specified type, ensuring that the correct sequence of tokens is processed.
   - The `BinaryExpr` class is used to create AST nodes that represent binary operations, including comparisons and membership tests.

## Example Usage

Here’s how you might use this function within the context of the parser:

```cpp
auto rootNode = parseComparison();
if (rootNode)
{
    // Process the parsed AST node
}
else
{
    // Handle parsing error
}
```

In this example, `parseComparison()` is called to parse a comparison expression starting at the current token position. The returned AST node can then be analyzed or compiled further depending on the needs of the Quantum Language compiler.

This function is crucial for accurately parsing complex expressions involving comparisons and membership tests, ensuring that they are correctly represented in the AST for subsequent compilation steps.