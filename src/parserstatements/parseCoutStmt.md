# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements that involve outputting data to the console using the `cout` keyword. This function constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input tokens.

## Parameters and Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but implied to be part of the broader parser context which includes the ability to access and consume tokens (`current()` and `consume()`).

- **Return Value**:
  - Returns a unique pointer to an ASTNode object containing a PrintStmt. The PrintStmt itself holds a vector of ASTNodePtrs representing the expressions to be printed and a boolean indicating whether a newline should be added after printing.

## How It Works

The function aims to correctly parse print statements involving multiple segments separated by the `<<` operator. Here's how it achieves this:

1. **Initialization**: 
   - It starts by recording the current line number (`ln`) where the parsing begins.
   - An empty vector `args` is initialized to store the parsed expressions.
   - A boolean flag `newline` is set to `false`, indicating whether a newline should be added after the print statement.

2. **Parsing Expressions**:
   - The function enters a loop where it repeatedly parses expressions to be printed.
   - Inside the loop, it first skips any newlines (`skipNewlines()`).
   - It checks if the next token is a left shift operator (`TokenType::LSHIFT`). If not, it breaks out of the loop, assuming all expressions have been parsed.
   - When encountering a left shift operator, it consumes the token (`consume()`). This ensures that the `<<` operator is treated as a stream insertion separator rather than a bitwise shift operator.

3. **Handling Special Case: `endl`**:
   - If the next token is an identifier (`TokenType::IDENTIFIER`) and its value is `"endl"`, it consumes the token and sets the `newline` flag to `true`. This indicates that a newline character should be added after the print statement.
   - If the token is not `"endl"`, the function proceeds to parse the next expression at the precedence level of addition and subtraction (`parseAddSub()`).

4. **Storing Expressions**:
   - Each parsed expression is stored in the `args` vector. However, if the expression is a string literal ending with `\n`, the `newline` flag is set to `true` instead of storing the expression. This allows for handling strings that contain newlines directly without needing to use `endl`.

5. **Finalization**:
   - After exiting the loop, the function continues to consume any remaining newlines or semicolons (`while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON)) consume();`). This ensures that the entire print statement is fully consumed, including any trailing whitespace or punctuation.
   - Finally, it returns a unique pointer to an ASTNode object containing a PrintStmt. The PrintStmt has the collected arguments and the newline flag, ready to be used in further compilation steps.

## Edge Cases

- **Multiple Expressions**: The function can handle multiple expressions separated by `<<`.
- **String Literals Ending with `\n`**: Strings that end with `\n` are treated as special cases, allowing them to trigger a newline without needing `endl`.
- **No Expressions**: If there are no expressions to print (i.e., only `cout << endl;`), the function will still produce a valid ASTNode with the `newline` flag set to `true`.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. Tokens such as `cout`, `<<`, `endl`, identifiers, and literals are essential for its operation.
- **Precedence Parsing**: By calling `parseAddSub()` instead of `parseExpr()`, the function ensures that expressions within the print statement are parsed according to the correct precedence rules, preventing issues like unintended bitwise shifts.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely interacts with error handling mechanisms to report any parsing errors or unexpected tokens encountered during the process.