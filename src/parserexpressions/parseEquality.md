# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is responsible for parsing equality expressions from the source code. This includes both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons. The function treats strict equality (`===`, `!==`) as equivalent to their non-strict counterparts (`==`, `!=`) because Quantum is a dynamically typed language.

## Parameters/Return Value

### Parameters
- None explicitly mentioned in the provided code snippet.

### Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed equality expression.

## How It Works

1. **Initial Parsing**: The function starts by calling `parseComparison()` to parse the left-hand side of the equality expression. This function returns a unique pointer to an `ASTNode`.

2. **Loop for Equality Operators**: 
   - The function enters a loop that continues as long as the next token is one of the equality operators (`TokenType::EQ`, `TokenType::NEQ`, `TokenType::STRICT_EQ`, or `TokenType::STRICT_NEQ`).
   
3. **Token Consumption**:
   - Inside the loop, the function consumes the current token using `consume()`. This token represents the equality operator encountered.
   
4. **Operator String Conversion**:
   - Depending on the type of the consumed token, the function converts it into a string representation of the operator. For strict equality (`===`), it uses `"=="`, and for strict inequality (`!==`), it uses `"!="`. For loose equality (`==`) and loose inequality (`!=`), it directly uses the values of the tokens.

5. **Right-hand Side Parsing**:
   - After consuming the operator token, the function calls `parseComparison()` again to parse the right-hand side of the equality expression.

6. **Creating ASTNode**:
   - The function then creates a new `ASTNode` containing a `BinaryExpr` object. This `BinaryExpr` object encapsulates the operator string and the two parsed sub-expressions (left and right). The line number of the current token is passed to the `ASTNode` constructor.

7. **Updating Left-hand Side**:
   - The newly created `ASTNode` becomes the new left-hand side of the expression, and the loop continues until there are no more equality operators.

8. **Exit Loop**:
   - Once the loop exits, the function returns the final `ASTNode` which represents the entire equality expression.

## Edge Cases

- **No Equality Operator**: If there are no equality operators after the initial call to `parseComparison()`, the function simply returns the result of `parseComparison()`.
  
- **Multiple Consecutive Equality Operators**: The function can handle multiple consecutive equality operators correctly, building up the expression tree accordingly.

- **Mixed Types**: Since Quantum is dynamically typed, the function ensures that the types of the operands are compatible before performing the comparison.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the correct sequence of tokens, including the equality operators.
  
- **ASTBuilder**: The function constructs an abstract syntax tree (AST) node for each equality expression, which is then used by other parts of the compiler for further processing, such as semantic analysis and code generation.

This implementation ensures that equality expressions are correctly parsed and represented in the AST, facilitating subsequent stages of compilation.