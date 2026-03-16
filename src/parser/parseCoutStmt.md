# parseCoutStmt

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements involving output operations using the `cout` object. It specifically handles the syntax of `cout` statements where multiple expressions can be concatenated and separated by the `<<` operator. The function ensures that each `<<` remains available as a stream insertion separator rather than being consumed as a bitwise shift operator.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: Returns a unique pointer to an `ASTNode` representing the parsed `cout` statement.

## How It Works

The function starts by recording the current line number (`ln`) for error reporting purposes. It then initializes an empty vector `args` to store the parsed expressions and a boolean flag `newline` to indicate whether a newline should be inserted after the output.

### Parsing Loop

The function enters a loop that continues until it encounters a token that is not `TokenType::LSHIFT`, which represents the `<<` operator. Within the loop:

1. **Skip Newlines**: Calls `skipNewlines()` to handle any leading newlines or spaces.
2. **Check for `<<` Operator**: Checks if the current token is `TokenType::LSHIFT`. If not, breaks out of the loop.
3. **Consume `<<` Token**: Consumes the `<<` token to move past it in the input stream.
4. **Handle `endl`**: If the next token is an identifier and its value is `"endl"`, sets the `newline` flag to `true` and consumes the token. This indicates that a newline should be inserted after the output.
5. **Parse Expressions**: If the next token is not `"endl"`, calls `parseAddSub()` to parse the next expression at the add/sub precedence level. This ensures that `<<` operators are treated correctly as stream insertion separators.
6. **Store Expressions**: If the parsed expression is a string literal ending with `\n`, sets the `newline` flag to `true`. Otherwise, adds the expression to the `args` vector.

### Post-Parsing Handling

After exiting the loop, the function checks for trailing newlines or semicolons and consumes them to ensure proper parsing. Finally, it returns a unique pointer to an `ASTNode` containing a `PrintStmt` object, which encapsulates the parsed arguments and the newline flag.

## Edge Cases

- **Empty `cout` Statement**: If the input stream contains only a `cout` followed by a semicolon, the function will return an empty `PrintStmt`.
- **String Literals Ending with `\n`**: String literals that end with `\n` are treated as special cases, indicating that a newline should be inserted after the output.
- **No Output Arguments**: If there are no valid output arguments (e.g., only `endl` without preceding expressions), the function will still return a `PrintStmt` with the `newline` flag set to `true`.

## Interactions with Other Components

- **Lexical Analyzer**: The function relies on the lexical analyzer to provide tokens such as `TokenType::LSHIFT`, `TokenType::IDENTIFIER`, and string literals.
- **Precedence Parser**: The use of `parseAddSub()` demonstrates the interaction with the precedence parser, ensuring that lower-precedence operators like `+` and `-` are correctly handled before higher-precedence operators like `<<`.
- **Error Reporting**: By keeping track of the current line number (`ln`), the function facilitates accurate error reporting if issues arise during parsing.