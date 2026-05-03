# `parseExpr` Function

## Overview

The `parseExpr` function is a critical component of the Quantum Language compiler's parser module. It is responsible for parsing expressions within the language, which can include literals, variables, and more complex constructs like arithmetic operations or function calls. The primary purpose of this function is to convert the abstract syntax tree (AST) nodes representing expressions into executable code.

### Why It Works This Way

The design of `parseExpr` as a wrapper around `parseAssignment()` is based on the fact that in many programming languages, assignments are treated as expressions. For example, in C++, an expression like `a = b + c` is valid because the assignment itself evaluates to the value being assigned (`b + c`). By having `parseExpr()` call `parseAssignment()`, we ensure that all forms of expressions, including assignments, are handled consistently and correctly.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: An AST node representing the parsed expression.

### Edge Cases

1. **Single Literal**: If the input consists solely of a literal (e.g., `42`), `parseAssignment()` will correctly handle it and return an AST node for that literal.
2. **Variable Reference**: If the input is a variable reference (e.g., `x`), `parseAssignment()` will create an AST node for that variable.
3. **Complex Expressions**: If the input includes more complex structures such as arithmetic operations (e.g., `a + b * c`) or function calls (e.g., `func(x, y)`), `parseAssignment()` will recursively parse these sub-expressions and combine them into a single AST node.

### Interactions With Other Components

- **Lexer**: `parseExpr()` relies on the lexer to tokenize the input source code. The lexer breaks down the source code into individual tokens, which are then consumed by `parseExpr()`.
- **Symbol Table**: During the parsing process, `parseExpr()` may need to look up symbols (variables, functions) in the symbol table to determine their types and values.
- **Error Handling**: If an invalid expression is encountered, `parseExpr()` should raise appropriate errors to be caught and handled by the error reporting mechanism of the compiler.

In summary, the `parseExpr` function plays a vital role in the Quantum Language compiler by handling the parsing of various types of expressions. Its implementation leverages the capabilities of `parseAssignment()` to ensure consistency and correctness across different expression scenarios.