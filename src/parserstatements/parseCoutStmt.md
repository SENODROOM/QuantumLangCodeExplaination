# parseCoutStmt

## Purpose

The `parseCoutStmt` function in the Quantum Language compiler is designed to parse statements that involve outputting data to the console using the `cout` keyword. It constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## Parameters and Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an ASTNode object containing a PrintStmt node. The PrintStmt node includes a vector of arguments (`args`) and a boolean flag indicating whether a newline should be printed after the statement (`newline`).

## How It Works

The function parses a `cout` statement, which typically follows the format:
```cpp
cout << expr1 << expr2 << ... << endl;
```
Here's a step-by-step breakdown of how `parseCoutStmt` functions:

1. **Initialization**:
   - Retrieve the current line number (`ln`) where the parsing begins.
   - Initialize an empty vector (`args`) to store the expressions to be printed.
   - Set a boolean flag (`newline`) to `false`, which will be used to determine if a newline character should be included in the output.

2. **Parsing Expressions**:
   - Enter a loop that continues until a non-left shift (`<<`) token is encountered.
   - Skip any newlines in the input stream using `skipNewlines()`.
   - Check if the next token is a left shift (`<<`). If not, break out of the loop.
   - Consume the left shift token (`consume()`).
   - Check if the next token is an identifier and its value is `"endl"`. If it is, consume the token and set the `newline` flag to `true`. Continue to the next iteration of the loop.
   - If the next token is not `"endl"`, parse the expression at the add-subtraction precedence level using `parseAddSub()`. This ensures that the left shift operators are correctly interpreted as stream insertion separators rather than bitwise shift operators.
   - If the parsed expression is a string literal and its value is exactly `"\n"`, set the `newline` flag to `true`. Otherwise, push the expression into the `args` vector.

3. **Handling Newlines and Semicolons**:
   - After the main loop, continue consuming tokens until either a newline or a semicolon is encountered. These tokens indicate the end of the statement.

4. **Creating the AST Node**:
   - Construct a unique pointer to an ASTNode object representing a print statement.
   - The ASTNode contains a PrintStmt node, which has two members:
     - `args`: A vector of ASTNodePtr objects representing the expressions to be printed.
     - `newline`: A boolean flag indicating whether a newline should be printed after the statement.
   - Return the constructed ASTNode.

## Edge Cases

- **Empty Stream Insertion**: If there are no expressions between `cout` and `<<`, the function will still create a valid ASTNode with an empty `args` vector.
- **String Literal with `\n`**: If a string literal containing `"\n"` is passed to the stream, it will be treated as a newline trigger, setting the `newline` flag to `true`.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Precedence Parsing**: By calling `parseAddSub()`, the function leverages the existing precedence parsing mechanism to handle complex expressions involving addition, subtraction, etc., without interference from the left shift operator.
- **Error Handling**: Although not explicitly shown in the code snippet, error handling mechanisms within the parser ensure that invalid syntax results in appropriate error messages being generated.

This function is crucial for accurately interpreting and constructing print statements in the Quantum Language, ensuring that the output matches the intended behavior specified by the programmer.