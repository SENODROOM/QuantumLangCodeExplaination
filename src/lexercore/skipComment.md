# skipComment

The `skipComment` function is designed to handle comments in the source code being processed by the Quantum Language compiler. Comments are sections of code that are ignored by the compiler and serve as notes or explanations for the programmer.

## What It Does

The primary purpose of the `skipComment` function is to iterate through the source code starting from the current position (`pos`) until it encounters the end of a comment or reaches the end of the file. This ensures that the compiler skips over any comment text without processing it as part of the program logic.

## Why It Works This Way

This implementation works because it continuously advances the position (`pos`) in the source code string (`src`) until one of two conditions is met:
1. The end of the file is reached (`pos < src.size()`).
2. A newline character (`'\n'`) is encountered, which typically signifies the end of a line comment in many programming languages.

By using a `while` loop, the function efficiently skips over all characters within the comment until the termination condition is met. This approach minimizes unnecessary checks and operations, focusing solely on advancing the position until the desired end of the comment is found.

## Parameters/Return Value

- **Parameters**:
  - `pos`: An integer representing the current position in the source code string (`src`). This parameter is modified within the function to move past the comment.

- **Return Value**:
  - The function does not explicitly return a value. Instead, it modifies the `pos` parameter to reflect its new position after skipping the comment.

## Edge Cases

### Single-line Comment Endings
If the comment ends with a newline character (`'\n'`), the function will correctly stop at the end of the comment. For example, in a single-line comment like `// This is a comment\n`, the function will stop at `\n`.

### Multi-line Comment Endings
For multi-line comments, which often begin with `/*` and end with `*/`, this function alone cannot handle them since it only processes up to the next newline character. Handling multi-line comments would require additional logic to detect the closing delimiter.

### Empty Lines
Empty lines in the source code do not contain comments and should be skipped over. However, since this function only stops when encountering a newline character, it will naturally skip empty lines if they follow a comment.

### File Endings
If the comment spans multiple lines and the file ends before the closing delimiter (`*/`), the function will continue iterating until the end of the file is reached. This behavior is intentional and allows the function to gracefully handle incomplete comments at the end of the file.

## Interactions With Other Components

The `skipComment` function interacts primarily with the lexer component of the compiler. Lexers are responsible for breaking down the source code into tokens, and comments need to be identified and skipped during this process.

When called, `skipComment` is invoked by the lexer whenever it detects a comment marker (e.g., `//` for single-line comments or `/*` for multi-line comments). After calling `skipComment`, the lexer can safely proceed to the next token without processing the comment content.

Additionally, `skipComment` may interact indirectly with error handling mechanisms within the compiler. If an unexpected character is encountered while skipping a comment, it could indicate a syntax error, prompting appropriate error messages or recovery strategies.

Overall, the `skipComment` function plays a crucial role in ensuring that the lexer accurately identifies and ignores comments, allowing the compiler to focus on parsing valid program logic.