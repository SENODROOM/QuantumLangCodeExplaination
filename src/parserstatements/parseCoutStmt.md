# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is responsible for parsing statements that involve outputting data to the console using the `cout` keyword. It constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## Functionality

The function parses a sequence of expressions and strings to be printed to the console. It handles both direct expressions and special identifiers like `endl`, which indicates a newline should be printed. The function ensures that the `<<` operator is correctly recognized as the stream insertion operator rather than a bitwise shift operator by calling `parseAddSub()` instead of `parseExpr()`.

### Detailed Steps:

1. **Initialization**: 
   - Retrieve the current line number (`ln`) where the statement starts.
   - Initialize an empty vector `args` to store the parsed expressions and strings.
   - Set a boolean flag `newline` to `false`.

2. **Parsing Expressions**:
   - Enter a loop that continues until a non-left shift token (`<<`) is encountered.
   - Skip any newlines using `skipNewlines()`.
   - Check if the current token is a left shift (`<<`). If not, break out of the loop.
   - Consume the left shift token (`consume()`).

3. **Handling Special Identifiers**:
   - If the next token is an identifier and its value is `endl`, consume the token and set `newline` to `true`. This indicates that a newline character should be printed.
   - Continue to the next iteration of the loop.

4. **Parsing Regular Expressions**:
   - For regular expressions, call `parseAddSub()` to ensure that the `<<` operator is treated as a stream insertion operator rather than a bitwise shift operator.
   - Check if the parsed expression is a string literal and if its value ends with `\n`. If so, set `newline` to `true`.
   - Otherwise, push the parsed expression into the `args` vector.

5. **Finalizing Parsing**:
   - After exiting the loop, consume any remaining newlines or semicolons using `while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON)) consume();`.
   - Create and return a unique pointer to an AST node representing the print statement. The node contains the parsed arguments and a boolean indicating whether a newline should be printed.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.

- **Return Value**:
  - Returns a unique pointer to an AST node of type `PrintStmt`, containing the parsed arguments and a boolean indicating whether a newline should be printed.

## Edge Cases

- **Empty Statement**: If there are no expressions or strings to print, the function will return a `PrintStmt` with an empty `args` vector and `newline` set to `false`.
- **String Literal Ending with `\n`**: If a string literal ends with `\n`, it is considered as a newline trigger and `newline` is set to `true`.
- **Bare `\n` String**: A bare string literal with only `\n` is treated as `endl` and `newline` is set to `true`.

## Interactions with Other Components

- **Tokenizer**: The function interacts with the tokenizer to retrieve the current token and check its type.
- **Error Handling**: While not shown in the provided code snippet, error handling mechanisms are likely integrated within the parser to manage unexpected tokens or syntax errors.
- **Abstract Syntax Tree (AST)**: The function constructs an AST node of type `PrintStmt`, which represents the parsed print statement. This node is then used by the compiler's subsequent stages to generate executable code.

By ensuring that the `<<` operator is correctly recognized as the stream insertion operator and handling special identifiers like `endl`, the `parseCoutStmt` function facilitates accurate parsing of console output statements in the Quantum Language.