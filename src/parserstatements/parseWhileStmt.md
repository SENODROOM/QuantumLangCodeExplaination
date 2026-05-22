# `parseWhileStmt`

The `parseWhileStmt` function is a crucial component of the Quantum Language compiler's parser, responsible for interpreting and converting while statements found within the source code into an Abstract Syntax Tree (AST). This AST serves as a foundational structure for subsequent compilation phases.

## What It Does

The function parses a while statement from the input source code and constructs an AST node representing that statement. The while statement typically consists of a condition expression followed by a block of code to be executed repeatedly as long as the condition remains true.

## Why It Works This Way

1. **Line Number Tracking**: The function starts by recording the line number (`ln`) where the while statement begins using `current().line`. This ensures that any errors or warnings related to the while statement can be accurately associated with its location in the source code.

2. **Condition Parsing**: The next step involves parsing the condition expression of the while loop. This is done using the `parseExpr()` function, which recursively breaks down the expression into smaller parts until it fully understands the logical condition.

3. **Colon Handling**: The function then checks for an optional Python-style colon (`:`) after the condition. While not strictly necessary in some languages, handling it here allows for compatibility with Python-like syntax, enhancing the flexibility of the compiler.

4. **Skipping Newlines**: After processing the colon, the function skips any newlines using `skipNewlines()`. This ensures that the parser can correctly handle multi-line while loops without misinterpreting them as separate statements.

5. **Body Parsing**: Finally, the function parses the body of the while loop. This is achieved by calling `parseBodyOrStatement()`, which can either parse a single statement or a block of statements enclosed in curly braces (`{}`). The parsed body is stored in the `body` variable.

6. **AST Node Creation**: With all components ready—condition, optional colon, skipped newlines, and body—the function creates a unique pointer to an `ASTNode` containing a `WhileStmt` object. This node encapsulates the entire while statement, including its condition and body, and returns it.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but implicitly relies on global state managed by the parser, such as the current token and the ability to consume tokens.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` representing the parsed while statement. The `ASTNode` contains a `WhileStmt` object, which holds the parsed condition and body.

## Edge Cases

1. **Missing Colon**: If the while statement does not end with a colon, the function will still proceed to parse the body. However, this could lead to incorrect syntax if the body is intended to be a single statement.
   
2. **Empty Body**: A while loop can have an empty body, represented by an empty block `{}`. The function should correctly handle this case and ensure that the AST reflects an empty body.

3. **Nested Statements**: The body of a while loop can contain nested statements or blocks. The function should be able to correctly parse these nested structures.

## Interactions with Other Components

- **Tokenizer**: The function interacts with the tokenizer to retrieve the current token and advance through the token stream during parsing.
  
- **Error Handling**: The function may rely on error handling mechanisms provided by the parser to report syntax errors or unexpected tokens encountered during the parsing process.

- **Scope Management**: Depending on the implementation, the function might interact with scope management components to ensure that variables declared within the while loop are properly scoped and accessible.

Overall, the `parseWhileStmt` function plays a vital role in the Quantum Language compiler's parsing phase, ensuring that while statements are accurately converted into an AST that can be used for further compilation steps.