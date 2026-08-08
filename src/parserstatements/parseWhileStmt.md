# `parseWhileStmt`

The `parseWhileStmt` function is a key part of the Quantum Language compiler's parser, tasked with translating while statements from the source code into an Abstract Syntax Tree (AST). The generated AST forms the backbone for further stages of compilation.

## What It Does

The primary role of `parseWhileStmt` is to convert a while statement in the source code into an equivalent AST node. A while statement typically consists of a condition and a block of code that executes repeatedly as long as the condition remains true.

Here’s how it breaks down:
1. **Condition Parsing**: The function starts by parsing the condition expression using `parseExpr()`. This expression must evaluate to a boolean value.
2. **Colon Handling**: It then checks for an optional colon (`:`) at the end of the condition line. In some languages, like Python, the colon is used to denote the start of the block following the condition.
3. **Skipping Newlines**: After handling the colon, the function skips any newline characters to ensure the parser continues reading the next part of the statement or block.
4. **Body Parsing**: Finally, it parses the body of the while loop using `parseBodyOrStatement()`, which can handle either a single statement or a block of multiple statements enclosed within curly braces (`{}`).

The parsed while statement is encapsulated into an `ASTNode` containing a `WhileStmt` object, which holds both the condition and the body. Additionally, the function records the line number where the while statement begins using `current().line`.

## Why It Works This Way

This design ensures that the parser correctly handles various syntax variations while maintaining consistency across different programming paradigms. By separating the parsing of the condition and the body, the function allows for flexibility in how these parts are structured in the source code.

- **Optional Colon**: Allowing an optional colon aligns with Pythonic conventions but doesn’t impose restrictions on other languages that might not use this syntax.
- **Line Number Recording**: Keeping track of the line number helps in error reporting and debugging, providing context when issues arise during the compilation process.

## Parameters/Return Value

### Parameters

- None explicitly listed in the provided code snippet.

### Return Value

- `std::unique_ptr<ASTNode>`: Returns a unique pointer to an `ASTNode` representing the parsed while statement. The `ASTNode` contains a `WhileStmt` object with the parsed condition and body.

## Edge Cases

- **Empty Body**: If the body of the while loop is empty, the `parseBodyOrStatement()` function will still return a valid AST node, likely representing an empty block or a placeholder.
- **Nested Loops**: The function can be nested within itself or other control structures, allowing for complex looping constructs.
- **Syntax Errors**: If the syntax of the while statement is incorrect (e.g., missing condition or body), the parser will throw an appropriate error, facilitating early detection and correction of issues.

## Interactions With Other Components

- **Lexer**: The function relies on the lexer to tokenize the input source code. It uses functions like `current()` to access the current token and `match()` to consume tokens based on their type.
- **Error Reporting**: During parsing, if any unexpected token or syntax error is encountered, the parser reports an error, which is crucial for debugging and ensuring the quality of the input source code.
- **Code Generation**: Once the AST is constructed, it is passed to subsequent phases such as code generation, where the actual machine code or quantum circuit instructions are derived from the AST structure.

In summary, the `parseWhileStmt` function efficiently translates while statements into an AST, enabling seamless integration with other compiler components and supporting a wide range of syntax variations.