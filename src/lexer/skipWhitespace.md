# skipWhitespace Function - Whitespace Consumption

## Overview
The `skipWhitespace()` function efficiently consumes and ignores whitespace characters (spaces, tabs, and carriage returns) in the source code. This is essential for lexical analysis as it allows the lexer to skip over formatting characters without affecting the parsing of meaningful tokens.

## Parameters
- None

## Return Value
- None

## How It Works
The function iterates through the source code starting from the current position (`pos`). During each iteration, it checks if the character at the current position is a space (' '), tab ('\t'), or carriage return ('\r'). If any of these conditions are met, the function advances the position pointer (`advance()`) to move past the whitespace character(s). This process continues until a non-whitespace character is encountered or the end of the source code is reached.

### Why It Works This Way
This method ensures that all leading and trailing whitespace characters are ignored during lexical analysis. By consuming spaces, tabs, and carriage returns, the lexer can focus on processing significant characters that define the structure and content of the program. This approach simplifies tokenization and reduces complexity in subsequent stages of compilation.

## Edge Cases
1. **Empty Source Code**: If the source code is empty (`src.size() == 0`), the function will not perform any operations since there are no characters to consume.
2. **No Whitespace**: If there is no whitespace between tokens, the function will simply iterate through the source code until it encounters a non-whitespace character, effectively skipping over any characters that do not contribute to the formation of tokens.
3. **End of Line**: The function correctly handles different types of whitespace characters, including line breaks, ensuring that the lexer can accurately track the position within the source code even when encountering new lines.

## Interactions With Other Components
The `skipWhitespace()` function interacts closely with the lexer's state management. It relies on the `pos` variable to keep track of the current position within the source code and uses the `current()` method to access the character at the current position. After advancing the position pointer, the lexer can proceed to identify and process the next significant token. Additionally, this function may be called before token recognition to ensure that the lexer starts processing from the correct position, ignoring any unnecessary whitespace.

In summary, the `skipWhitespace()` function plays a crucial role in the quantum language compiler by efficiently consuming and ignoring whitespace characters. Its implementation ensures accurate lexical analysis and simplifies the overall parsing process.