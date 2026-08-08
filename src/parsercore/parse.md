# `parse` Function

## Overview
The `parse` function is the central component responsible for interpreting and converting a sequence of lexical tokens into an Abstract Syntax Tree (AST) in the Quantum Language compiler. This process enables the syntactic analysis and comprehension of quantum programs, facilitating their subsequent compilation or interpretation phases.

### Why It Works This Way
The `parse` function operates by constructing a `BlockStmt`, which represents the top-level structure of a quantum program. The function iterates through the lexical tokens using a loop, parsing each token into a statement (`parseStatement`) and appending it to the `statements` vector within the `BlockStmt`. By skipping newlines after each parsed statement (`skipNewlines()`), the function ensures that the AST accurately reflects the intended structure of the quantum program, even when there are multiple statements on a single line or separated by blank lines.

## Parameters/Return Value
- **Parameters**: None explicitly defined in the provided code snippet; however, it relies on an external state or context that provides access to the current lexical tokens and determines whether the end of input has been reached (`atEnd()`).
- **Return Value**: A unique pointer to an `ASTNode` representing the root of the parsed AST.

## Edge Cases
- **Empty Program**: If the input contains no lexical tokens, the function will return an empty `BlockStmt`.
- **Single Statement**: If the input consists of a single statement followed by optional whitespace, the function will correctly parse and return a `BlockStmt` containing only that statement.
- **Multiple Statements**: The function can handle multiple statements, each potentially spanning multiple lines, and will correctly construct a `BlockStmt` with all statements included.

## Interactions with Other Components
- **Lexer**: The `parse` function relies on the lexer to provide a stream of lexical tokens. The lexer's output is used to drive the parsing process.
- **Token Stream**: The function maintains an implicit reference to the current position in the token stream, advancing through tokens as they are parsed.
- **Error Handling**: While not shown in the provided code snippet, the `parse` function should interact with error handling mechanisms to report syntax errors encountered during parsing.

By leveraging these components, the `parse` function effectively transforms the raw text representation of a quantum program into a structured AST, enabling further processing and analysis within the compiler.