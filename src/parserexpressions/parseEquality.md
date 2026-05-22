# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons, treating strict equality (`===`, `!==`) as their non-strict counterparts (`==`, `!=`). This simplifies the parsing process by reducing the number of distinct operators that need to be handled.

## Parameters/Return Value

### Parameters

- None

### Return Value

- `std::unique_ptr<ASTNode>`: A unique pointer to an ASTNode representing the parsed equality expression.

## How It Works

1. **Initialization**: The function starts by calling `parseComparison()` to parse the left-hand side of the equality expression. This function returns an `std::unique_ptr<ASTNode>` representing the comparison part of the expression.

2. **Loop Through Equality Operators**:
   - The function enters a loop that continues as long as the next token is one of the equality operators (`==`, `!=`, `===`, `!==`).
   - Inside the loop, it retrieves the line number (`ln`) where the operator occurs using `current().line`.
   - It consumes the current token (`op`) using `consume()`. Depending on whether the token type is `TokenType::STRICT_EQ` or `TokenType::STRICT_NEQ`, it sets `opStr` to `"=="` or `"!="`, respectively. For non-strict operators, `opStr` is set directly to the operator's value.

3. **Parse Right-Hand Side**:
   - After consuming the operator, the function calls `parseComparison()` again to parse the right-hand side of the equality expression.

4. **Create Binary Expression Node**:
   - The function creates a new `BinaryExpr` node with the operator string (`opStr`), the left-hand side (`left`), and the right-hand side (`right`). It also assigns the line number (`ln`) to the new node.
   - The new `BinaryExpr` node is wrapped in a `std::unique_ptr<ASTNode>` and assigned back to `left`.

5. **Repeat Loop**:
   - The loop repeats until there are no more equality operators following the current expression.

6. **Return Result**:
   - Once the loop exits, the function returns the final `left` node, which now represents the entire equality expression tree.

## Edge Cases

- **No Equality Operators**: If there are no equality operators after the initial comparison, the function simply returns the result of `parseComparison()`.
- **Nested Equality Expressions**: The function can handle nested equality expressions, such as `a == b != c`. In this case, it will correctly parse the inner comparison first (`b != c`), then combine it with the outer comparison (`a == ...`).

## Interactions With Other Components

- **Parsing Context**: The `parseEquality` function operates within the broader context of the parser, utilizing helper functions like `check()` and `consume()` to navigate through the tokens in the source code.
- **Abstract Syntax Tree (AST)**: The function constructs an AST by creating `BinaryExpr` nodes. These nodes represent the structure of the equality expressions and are used by subsequent stages of the compiler for semantic analysis and code generation.
- **Error Handling**: Although not explicitly shown in the provided code snippet, the parser likely includes error handling mechanisms to manage unexpected tokens or syntax errors during the parsing process.

This implementation ensures that the Quantum Language Compiler can accurately parse equality expressions, taking into account both strict and loose comparisons, while maintaining simplicity and efficiency in the parsing logic.