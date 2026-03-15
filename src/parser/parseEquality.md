# `parseEquality` Function

The `parseEquality` function in the Quantum Language compiler is responsible for parsing equality and inequality expressions. It builds an Abstract Syntax Tree (AST) node representing these operations.

## What it Does

This function parses expressions that involve equality (`==`) or inequality (`!=`). It starts by calling `parseComparison()` to get the left-hand side of the expression. Then, it enters a loop that continues as long as the next token is one of the following types:

- `TokenType::EQ`: Standard equality operator (`==`)
- `TokenType::NEQ`: Standard inequality operator (`!=`)
- `TokenType::STRICT_EQ`: Strict equality operator (`===`)
- `TokenType::STRICT_NEQ`: Strict inequality operator (`!==`)

For each iteration of the loop, it consumes the current token (which represents the equality or inequality operator) and recursively calls `parseComparison()` again to get the right-hand side of the expression. The resulting binary operation is then wrapped in an `ASTNode`, which becomes the new left-hand side for the next iteration or the final result if there are no more equality or inequality operators.

## Why it Works This Way

The function works this way because it leverages the recursive descent parsing technique, where smaller parts of the grammar are parsed first before combining them into larger structures. In this case, `parseComparison()` handles the comparison part of the expression, and `parseEquality` extends that functionality to include equality and inequality checks.

By treating strict equality (`===`) and strict inequality (`!==`) as equivalent to standard equality (`==`) and inequality (`!=`), respectively, the function simplifies the AST generation process without losing any information. This is particularly useful in dynamically typed languages like Quantum, where type checking can be deferred until runtime.

## Parameters/Return Value

### Parameters

- None explicitly declared in the function signature; it uses global state managed by the parser.

### Return Value

- Returns a `std::unique_ptr<ASTNode>` representing the root of the parsed equality or inequality expression tree.

## Edge Cases

1. **No Equality or Inequality Operators**: If the input does not contain any equality or inequality operators after the initial call to `parseComparison()`, the function simply returns the result of `parseComparison()`.
2. **Nested Expressions**: The function correctly handles nested equality and inequality expressions by recursively calling itself.
3. **End of Input**: If the end of the input stream is reached during parsing, the function will handle it gracefully, returning whatever has been parsed up to that point.

## Interactions with Other Components

- **Lexical Analyzer**: The function relies on the lexical analyzer to tokenize the input source code.
- **Error Handling**: While not shown in the snippet, the function likely interacts with error handling mechanisms provided by the parser to manage syntax errors.
- **AST Construction**: The function constructs an AST using classes such as `BinaryExpr` and `ASTNode`. These classes are presumably defined elsewhere in the compiler's codebase.
- **Token Consumption**: The function consumes tokens using methods like `current()` and `consume()`, which are part of the parser's internal state management.

Overall, `parseEquality` plays a crucial role in the parser's ability to construct a complete AST for quantum programs, enabling further compilation steps such as semantic analysis and code generation.