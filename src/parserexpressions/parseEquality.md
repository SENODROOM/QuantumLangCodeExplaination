# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons, treating strict equality (`===`, `!==`) as their non-strict counterparts (`==`, `!=`). This simplification aligns with the dynamic typing nature of the Quantum language, where type coercion can lead to unexpected results.

## Parameters

- **None**: This function operates directly on the global parser state without requiring any explicit parameters.

## Return Value

- **std::unique_ptr<ASTNode>**: The function returns an abstract syntax tree (AST) node representing the parsed equality expression. If no equality operators are found after parsing the initial comparison, it simply returns the result of `parseComparison()`.

## How It Works

1. **Initial Parsing**: The function starts by calling `parseComparison()`, which parses the left-hand side of the equality expression.
2. **Loop for Equality Operators**: It then enters a loop that continues as long as the next token is one of the equality operators (`==`, `!=`, `===`, `!==`).
3. **Token Consumption**: Inside the loop, the function consumes the current token using `consume()`. This token represents the equality operator.
4. **Operator String Conversion**: Depending on whether the consumed token is a strict equality or inequality operator, the function converts it to its non-strict counterpart (`"=="` for `"==="`, `"!="` for `"!=="`). This conversion is done to simplify the AST generation process.
5. **Right-Hand Side Parsing**: The function then calls `parseComparison()` again to parse the right-hand side of the equality expression.
6. **Binary Expression Node Creation**: A new `BinaryExpr` node is created with the converted operator string, the left-hand side AST node, and the right-hand side AST node. This node represents the entire equality expression.
7. **Line Number Tracking**: The line number (`ln`) of the current token is tracked and passed to the `ASTNode` constructor to maintain accurate error reporting and debugging information.
8. **Return Statement**: Once the loop exits (i.e., no more equality operators are found), the function returns the final AST node representing the parsed equality expression.

## Edge Cases

- **No Equality Operators**: If there are no equality operators following the initial comparison, the function will simply return the result of `parseComparison()`.
- **Strict vs Non-Strict Comparison**: The function treats strict equality and inequality operators as their non-strict counterparts, which might lead to different behavior depending on the context of type coercion in the Quantum language.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: During parsing, the function may encounter invalid tokens or unexpected end-of-file conditions, which would be handled by the broader error handling mechanism of the compiler.
- **Abstract Syntax Tree (AST)**: The function constructs and manipulates AST nodes, which are used to represent the syntactic structure of the parsed code. These nodes are further processed by subsequent stages of the compiler to generate executable code or intermediate representations.