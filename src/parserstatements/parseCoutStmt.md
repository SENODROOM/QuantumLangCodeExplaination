# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements that involve outputting data to the console using the `cout` keyword. This function constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input. The primary goal of this function is to handle complex expressions involving multiple insertions (`<<`) and ensure that the correct AST structure is generated.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but implicitly uses global state such as the current token and methods like `skipNewlines()` and `consume()`.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` object containing a `PrintStmt`. This node represents the parsed print statement, including its arguments and whether a newline should be printed after the statement.

## How It Works

The function begins by capturing the current line number where the `cout` statement starts. It then initializes an empty vector `args` to store the expression segments that will be inserted into the output stream, and a boolean flag `newline` to indicate if a newline character should be included.

The function enters a loop where it repeatedly checks for the `TokenType::LSHIFT`, which corresponds to the `<<` operator used in stream insertion. If `<<` is found, it consumes the token and proceeds to parse the next expression segment. 

### Handling Expressions and Newlines

- **Expression Parsing**: Inside the loop, the function calls `parseAddSub()`, which parses expressions based on the addition and subtraction precedence. This ensures that the `<<` operator is not consumed prematurely by higher-precedence operators like bitwise shifts.

- **String Literal Check**: After parsing an expression, the function checks if the expression is a string literal. If the string is exactly `"endl"`, it sets the `newline` flag to `true` and continues to the next iteration without adding the expression to the `args` vector. This allows the `endl` identifier to act as a special case, triggering a newline in the output.

- **Appending Non-String Expressions**: If the parsed expression is not a string literal, it is appended to the `args` vector. This handles all other types of expressions that can be streamed into the output.

### Loop Exit Condition

The loop exits when `<<` is no longer found, indicating that all segments have been parsed. The function then skips any remaining newlines or semicolons that might follow the print statement.

### Creating the AST Node

Finally, the function creates an `ASTNode` containing a `PrintStmt` object. This object includes the list of parsed expression segments (`args`) and the `newline` flag. The function returns this unique pointer to the newly created AST node.

## Edge Cases

- **Empty Expression Segments**: The function gracefully handles cases where there are no valid expression segments following `cout`. In such cases, it simply returns a print statement with no arguments and no newline trigger.

- **Mixed Types**: The function correctly handles mixed types of expression segments, appending them to the `args` vector regardless of their type (e.g., integers, strings, variables).

- **Trailing Semicolons/Newlines**: Any trailing newlines or semicolons after the print statement are skipped, ensuring that the parser remains consistent and does not misinterpret subsequent tokens.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens that represent the source code. It uses methods like `current()` and `consume()` to interact with the tokenizer's state.

- **Precedence Parser**: By calling `parseAddSub()` instead of `parseExpr()`, the function leverages the precedence parser to handle different levels of expression complexity, ensuring that the `<<` operator is treated as a stream insertion rather than a bitwise shift.

- **Error Handling**: Although not explicitly shown in the provided code, the function likely interacts with error handling mechanisms within the compiler to manage invalid input gracefully.

In summary, the `parseCoutStmt` function is crucial for accurately parsing print statements in the Quantum Language compiler. Its design ensures that complex expressions involving multiple stream insertions are handled correctly, and it provides a robust foundation for constructing an appropriate AST representation of these statements.