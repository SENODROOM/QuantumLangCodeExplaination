# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements that involve outputting data to the console using the `cout` keyword. This function constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## Parameters

- None

## Return Value

- Returns a unique pointer to an ASTNode object representing the parsed print statement.

## How It Works

The function parses a `cout` statement and constructs an AST node for it. The `cout` statement can include multiple expressions separated by `<<`, which is used as the stream insertion operator. The function ensures that `<<` is not consumed greedily by calling `parseAddSub()` instead of `parseExpr()`. This allows each `<<` to stay available as the stream insertion separator.

Here's a step-by-step breakdown of how the function works:

1. **Initialization**: 
   - The function starts by getting the current line number (`ln`) where the `cout` statement begins.
   - An empty vector `args` is initialized to store the expressions to be printed.
   - A boolean variable `newline` is set to `false` to indicate whether a newline should be added after printing.

2. **Parsing Expressions**:
   - The function enters a loop that continues until it encounters a token that is not `<<`.
   - Inside the loop, it skips any newlines using `skipNewlines()`.
   - It checks if the current token is `<<`. If it is, the function consumes the `<<` token.
   - If the next token is an identifier with the value `"endl"`, the function consumes it and sets `newline` to `true`.
   - Otherwise, the function calls `parseAddSub()` to parse the next expression at the add/sub precedence level. This ensures that `<<` is not consumed greedily by `parseExpr()`.
   - If the parsed expression is a string literal ending with `\n`, the function sets `newline` to `true`. Otherwise, it adds the expression to the `args` vector.

3. **Consuming Newlines or Semicolons**:
   - After parsing all expressions, the function enters another loop that continues until it encounters either a newline (`TokenType::NEWLINE`) or a semicolon (`TokenType::SEMICOLON`). These tokens are consumed to ensure proper parsing of the statement.

4. **Constructing the AST Node**:
   - Finally, the function returns a unique pointer to an ASTNode object representing the parsed print statement. The ASTNode contains a `PrintStmt` object, which includes the list of arguments (`args`) and a boolean indicating whether a newline should be added (`newline`).

## Edge Cases

- **Empty Expression List**: If there are no expressions following `cout`, the function will still construct a valid AST node with an empty list of arguments.
- **String Literal Ending with `\n`**: If a string literal ends with `\n`, it is treated as a special case where a newline should be added after printing, regardless of whether `endl` is explicitly mentioned.
- **Mixed Expressions**: The function can handle a mix of different types of expressions (e.g., integers, strings, variables) following `cout`.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. The tokenizer breaks down the source code into individual tokens, which are then processed by `parseCoutStmt`.
- **Error Handling**: While not shown in the provided code snippet, the function likely interacts with error handling mechanisms within the compiler to report any syntax errors encountered during parsing.
- **Precedence Parsing**: The use of `parseAddSub()` demonstrates the interaction with the precedence parsing mechanism, ensuring that operators like `+` and `-` are correctly handled when they appear in the same expression as `<<`.

This function is crucial for accurately parsing `cout` statements in the Quantum Language, allowing the compiler to generate the appropriate AST nodes for subsequent compilation steps.