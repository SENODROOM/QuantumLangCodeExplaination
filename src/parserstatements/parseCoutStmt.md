# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements that involve outputting data to the console using the `cout` keyword. This function constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## How It Works

The function processes a sequence of expressions and special tokens (`<<`, `endl`) to build a `PrintStmt` AST node. The key points of its operation include:

- **Avoiding Greedy Parsing**: The function avoids calling `parseExpr()` directly because doing so would cause the parser to greedily consume the `<<` token as a bitwise shift operator. Instead, it uses `parseAddSub()`, which is one level below `parseExpr()`, ensuring that each `<<` remains available as the stream insertion separator.
  
- **Handling Expressions**: Inside the loop, the function checks for the `<<` token and consumes it when found. It then skips any newlines and parses the next expression at the `add/sub` precedence level. Each parsed expression is added to a vector called `args`.

- **Special Token Handling**: The function also checks for the `endl` identifier. When encountered, it consumes the token and sets a flag `newline` to true, indicating that a newline should be printed after these arguments.

- **Edge Case Handling**: The function handles edge cases such as strings that end with `\n`. In this case, the newline character is kept within the string literal itself, rather than being treated as a separate `endl` token.

- **Finalization**: After processing all valid segments, the function skips any remaining newlines or semicolons. Finally, it returns a unique pointer to a new `ASTNode` containing the `PrintStmt` with the collected arguments and the newline flag.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.

- **Return Value**:
  - Returns a `unique_ptr<ASTNode>` representing the parsed print statement.

## Edge Cases

- **Empty Stream**: If there are no expressions or tokens between `cout` and `endl`, the function will still correctly handle the situation by setting the `newline` flag to true.
- **String Literals Ending with `\n`**: The function treats strings like `"Hello\n"` differently from standalone `"endl"`. The newline character in the string is preserved, whereas a standalone `"endl"` triggers a newline.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token during parsing.
- **Precedence Parser**: By using `parseAddSub()`, it leverages the precedence parser to ensure correct handling of expressions with different levels of precedence.
- **Error Handling**: Although not shown in the snippet, the function likely interacts with error handling mechanisms to manage unexpected tokens or malformed statements.

This function plays a crucial role in accurately parsing print statements in the Quantum Language, ensuring that the resulting AST can be used for further compilation or interpretation tasks.