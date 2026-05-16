# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons, treating strict equality (`===`, `!==`) as their non-strict counterparts (`==`, `!=`). This behavior aligns with the dynamic typing nature of the Quantum language, where type coercion can lead to unexpected results.

## Parameters

- **None**: The function does not take any explicit parameters.

## Return Value

- **std::unique_ptr<ASTNode>**: Returns an abstract syntax tree (AST) node representing the parsed equality expression. If no equality operators are found, it returns the result of parsing a comparison expression.

## Detailed Explanation

### Parsing Process

1. **Initial Comparison**:
   - The function starts by calling `parseComparison()` to get the left-hand side of the equality expression. This function parses expressions that include relational operators like `<`, `<=`, `>`, `>=`.

2. **Loop for Equality Operators**:
   - Using a `while` loop, the function checks for the presence of any equality operator (`==`, `!=`, `===`, `!==`). The loop continues as long as one of these operators is found.

3. **Operator Consumption**:
   - Inside the loop, the function consumes the current token using `consume()`. This token represents the equality or inequality operator encountered.

4. **Handling Strict vs Loose Equality**:
   - To handle strict and loose equality uniformly, the function converts the strict operators (`===`, `!==`) into their non-strict counterparts (`==`, `!=`). This is done using a conditional statement:
     ```cpp
     std::string opStr = (op.type == TokenType::STRICT_EQ) ? "==" 
                                                            : (op.type == TokenType::STRICT_NEQ) ? "!=" 
                                                                                                  : op.value;
     ```
   - Here, `op.value` directly holds the string representation of the operator when it's not strict.

5. **Right-hand Side Parsing**:
   - After consuming the operator, the function calls `parseComparison()` again to parse the right-hand side of the equality expression.

6. **Creating AST Node**:
   - With both sides of the expression parsed, the function creates a new `BinaryExpr` node in the AST. This node encapsulates the operator and the two operands (left and right). The line number (`ln`) is also recorded.

7. **Updating Left-hand Side**:
   - The newly created `BinaryExpr` node becomes the new left-hand side for the next iteration of the loop, allowing for consecutive equality operations to be chained together.

8. **Exit Loop**:
   - When no more equality operators are found, the loop exits, and the final AST node representing the entire equality expression chain is returned.

### Edge Cases

- **No Equality Operator**: If the input stream does not contain any equality operators after the initial comparison, the function simply returns the result of `parseComparison()`.
- **Nested Equality Expressions**: The function supports nested equality expressions, meaning you can have something like `a == b == c`. Each equality operation will be evaluated sequentially, creating a deeply nested `BinaryExpr` structure in the AST.

### Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. Tokens represent individual elements of the source code, such as keywords, identifiers, and operators.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function should integrate with the error handling mechanism of the parser to report syntax errors related to unexpected tokens or misplaced operators.
- **Abstract Syntax Tree (AST)**: The function constructs an AST, which is used for further stages of compilation, including semantic analysis and code generation. The `BinaryExpr` nodes created by this function serve as building blocks for the overall AST.

This function is crucial for correctly interpreting and constructing equality expressions in the Quantum language, ensuring that the semantics of the code are accurately represented in the AST.