# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is responsible for parsing statements that involve outputting data to the console using the `cout` keyword. It constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

### What It Does

The function parses a `cout` statement and extracts expressions to be printed. It handles both regular expressions and special tokens like `endl`, which indicates a newline should be printed after the expression. The resulting AST node contains a list of these expressions and a boolean flag indicating whether a newline should be added after printing.

### Why It Works This Way

The function avoids calling `parseExpr()` directly due to potential issues with the `<<` operator being interpreted as a bitwise shift rather than a stream insertion operator. By calling `parseAddSub()` instead, the function ensures that each `<<` remains available as the stream insertion separator, allowing for proper parsing of complex expressions involving multiple insertions.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: Returns a unique pointer to an `ASTNode` containing a `PrintStmt`. The `PrintStmt` includes a vector of `ASTNodePtr` representing the expressions to be printed and a boolean indicating whether a newline should be added.

### Edge Cases

- **Empty Expression List**: If there are no expressions to print, the function returns an empty `args` vector and sets `newline` to `false`.
- **String Literal with Newline**: If a string literal ends with `\n`, it is treated as a special case where `newline` is set to `true` without adding it to the `args` vector.
- **Bare Newline String**: A bare string literal `"endl"` is treated as a special token that triggers a newline, setting `newline` to `true`.

### Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token during parsing.
- **Error Handling**: While not explicitly shown in the code snippet, error handling mechanisms within the parser ensure that any invalid syntax or unexpected tokens result in appropriate error messages or exceptions.
- **Abstract Syntax Tree (AST)**: The function constructs and returns an `ASTNode` representing the parsed `cout` statement, which can then be used by subsequent stages of the compiler for further processing or code generation.