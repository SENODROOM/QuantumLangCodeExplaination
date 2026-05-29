# `parseWhileStmt`

The `parseWhileStmt` function is a key part of the Quantum Language compiler's parser, tasked with interpreting and transforming while statements from the source code into an Abstract Syntax Tree (AST). The AST acts as the backbone for further compilation steps.

## What It Does

The primary role of `parseWhileStmt` is to parse a while statement encountered in the source code. A while statement typically consists of a condition followed by a block of code that will be executed repeatedly as long as the condition remains true. The function extracts these elements and constructs an appropriate node in the AST.

## Why It Works This Way

### Step-by-Step Parsing

1. **Retrieve Current Line Number**: The function starts by retrieving the line number of the current token using `current().line`. This information is crucial for error reporting and maintaining the context of the parsed statement.

2. **Parse Condition**: The next step involves parsing the condition expression of the while loop. This is done by calling `parseExpr()`, which recursively parses expressions based on their complexity.

3. **Handle Optional Colon**: The parser then checks for an optional Python-style colon (`:`) following the condition. If present, it consumes the token using `match(TokenType::COLON)`.

4. **Skip Newlines**: After handling the colon, the function skips any newlines using `skipNewlines()`. This ensures that the parser can correctly identify the start of the loop body even if there are multiple blank lines between the condition and the body.

5. **Parse Body or Statement**: Finally, the function parses the body of the while loop. This is achieved by calling `parseBodyOrStatement()`, which handles both compound bodies (like blocks enclosed in curly braces `{}` or parentheses `()` in some languages) and single statements.

6. **Construct AST Node**: Once all components are parsed, the function constructs an `ASTNode` representing the while statement. This node contains the condition and the body, wrapped in a `WhileStmt` object. The constructed node is then returned along with the original line number.

### Error Handling and Context Management

By capturing the line number at the beginning of the function, `parseWhileStmt` ensures that any errors during parsing can be reported accurately. Additionally, skipping newlines helps maintain the correct flow and structure of the AST, even when the source code contains formatting inconsistencies.

## Parameters and Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet; however, it implicitly relies on global state managed by the parser, such as the current token and functions like `match` and `skipNewlines`.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` containing the parsed while statement node. The node encapsulates the condition and the body of the loop, providing a structured representation suitable for further processing.

## Edge Cases

- **Empty Condition**: If the condition expression cannot be parsed (e.g., due to syntax errors), the function should handle this gracefully, possibly throwing an exception or returning an error node.
  
- **Missing Body**: In some languages, a while loop might not have a body (e.g., `while (condition) ;`). The function should ensure that it correctly identifies and handles such cases, potentially creating an empty body node in the AST.

- **Nested Loops**: The parser must be capable of handling nested loops correctly, ensuring that each loop's body is properly associated with its respective condition.

## Interactions With Other Components

- **Lexer**: The function relies on the lexer to provide tokens for parsing. The lexer breaks down the source code into individual tokens, which are consumed by the parser.

- **Error Reporting**: By keeping track of the line number, `parseWhileStmt` facilitates accurate error reporting. When issues arise during parsing, the line number can be used to pinpoint where the problem occurred.

- **Scope Management**: While not explicitly shown in the provided code snippet, the parser likely interacts with scope management components to ensure that variables declared within the while loop are correctly handled and scoped.

In summary, `parseWhileStmt` is a vital function in the Quantum Language compiler's parser, responsible for accurately interpreting and constructing while statement nodes in the AST. Its design ensures robustness, flexibility, and integration with other critical components of the compiler.