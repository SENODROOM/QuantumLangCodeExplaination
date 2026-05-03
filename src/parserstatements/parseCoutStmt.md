# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements involving outputting data to the console using the `cout` keyword. It constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## Functionality

The function processes a sequence of expressions and special tokens (`<<`, `endl`) to build a `PrintStmt` AST node. The primary goal is to handle multiple expressions being inserted into the output stream correctly without misinterpreting them as bitwise shift operators.

### Key Steps

1. **Initialization**: 
   - Retrieves the current line number (`ln`) where the statement starts.
   - Initializes an empty vector `args` to store the expressions to be printed.
   - Sets a boolean flag `newline` to `false`, which will be used to indicate whether a newline should be included after the print operation.

2. **Loop to Parse Expressions**:
   - Continuously skips any newlines in the input.
   - Checks if the current token is a left shift operator (`<<`). If not, breaks out of the loop.
   - Consumes the left shift token to move past it.
   - If the next token is the identifier `endl`, consumes it and sets `newline` to `true`. This indicates that the end of the line should be explicitly handled.
   - Otherwise, parses the next expression at the add/sub precedence level using `parseAddSub()` and adds it to the `args` vector.

3. **Handling Newline or Semicolon**:
   - Skips any remaining newlines or semicolons in the input, ensuring that the statement ends properly.

4. **Return Statement**:
   - Constructs and returns a unique pointer to an `ASTNode` containing a `PrintStmt` object. This `PrintStmt` includes the collected arguments and the `newline` flag.

## Why It Works This Way

- **Avoiding Bitwise Shift Confusion**: By calling `parseAddSub()` instead of `parseExpr()`, the function ensures that the `<<` operator is treated as a stream insertion operator rather than a bitwise shift operator. This prevents premature consumption of `<<` tokens within nested expressions.
  
- **Flexible Output Handling**: The ability to include `endl` as a separate token allows for explicit control over when newlines are added to the output, providing flexibility in how print statements are formatted.

## Parameters/Return Value

- **Parameters**:
  - None directly passed to the function; it uses global state (current token, input stream) to determine what to parse.

- **Return Value**:
  - Returns a `unique_ptr<ASTNode>` containing a `PrintStmt` object. The `PrintStmt` encapsulates the list of expressions to be printed and a boolean indicating whether a newline should be appended.

## Edge Cases

- **Empty Print Statement**: If there are no expressions or special tokens after `cout`, the function will still construct a valid `PrintStmt` with an empty argument list.
- **String Literal with `\n`**: If a string literal containing `\n` is encountered, it is treated as a newline character and the `newline` flag is set accordingly.
- **Misuse of `endl`**: If `endl` is used outside of a `cout` context, it will be ignored, and the `newline` flag will remain `false`.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token in the input stream. The tokenizer's state is advanced through calls like `consume()` and `skipNewlines()`.
  
- **Precedence Parsing**: The use of `parseAddSub()` leverages the existing precedence parsing mechanism to handle expressions correctly. This integration ensures consistency with the rest of the language's parsing rules.

- **Error Handling**: While not shown in the snippet, the function likely interacts with error handling mechanisms to manage situations where invalid tokens are encountered during parsing.

This comprehensive approach ensures that `parseCoutStmt` can accurately parse complex print statements in the Quantum Language, maintaining both correctness and flexibility in output formatting.