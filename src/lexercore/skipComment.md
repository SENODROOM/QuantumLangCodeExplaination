# skipComment

The `skipComment` function is designed to handle comments in the source code being processed by the Quantum Language compiler. Comments are sections of code that are ignored by the compiler and serve as notes or explanations for the programmer.

## What It Does

The `skipComment` function advances the lexer's position through the input source until it encounters the end of the line, effectively skipping over any comment text. This function is crucial for parsing quantum language files where comments can appear anywhere within the code.

### Why It Works This Way

This approach ensures that the lexer skips over comments without affecting the subsequent parsing process. By advancing the position only until the newline character (`'\n'`) is encountered, the function maintains the integrity of the rest of the code structure, allowing the parser to continue processing from the correct location.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - The function returns nothing (`void`). It modifies the internal state of the lexer by changing its position.

## Edge Cases

1. **Single-line Comments**: If the comment starts at the beginning of the line, the function will correctly skip all characters up to the newline.
2. **Multi-line Comments**: Although the provided code snippet only handles single-line comments, the lexer should be designed to handle multi-line comments as well, which would involve different logic to detect the start and end of the comment block.
3. **Empty Lines**: If the line consists entirely of whitespace followed by a comment, the function will still correctly skip the entire line.

## Interactions With Other Components

- **Lexer Core**: The `skipComment` function is part of the Lexer Core component, responsible for breaking down the source code into tokens. After calling `skipComment`, the lexer continues processing the next token on the following line.
- **Error Handling**: While not explicitly shown in the snippet, the lexer should include error handling mechanisms to manage unexpected situations such as unterminated comments or invalid characters within comments.

By implementing the `skipComment` function as described, the Quantum Language compiler can accurately parse source code containing comments, ensuring that these elements do not interfere with the compilation process.