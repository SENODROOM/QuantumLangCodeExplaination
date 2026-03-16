# skipWhitespace Function - Whitespace Consumption

## Overview
The `skipWhitespace()` function is an integral part of the Quantum Language compiler's Lexer component. Its primary role is to consume and ignore whitespace characters such as spaces, tabs, and carriage returns from the input source code. This ensures that the lexer can accurately tokenize the meaningful content without being affected by formatting or layout issues.

## Parameters
- **None**: The function operates directly on the internal state of the Lexer object, specifically the `src` string and the `pos` cursor position.

## Return Value
- **void**: The function does not return any value; it modifies the internal state of the Lexer by advancing the cursor (`pos`) past the consumed whitespace characters.

## Edge Cases
1. **Empty Source Code**: If the source code string (`src`) is empty, the function will immediately return without performing any operations since there are no characters to process.
2. **Trailing Whitespace**: The function continues consuming whitespace until it encounters a non-whitespace character or reaches the end of the source code. This ensures that trailing whitespace does not interfere with subsequent tokenization.
3. **Mixed Whitespace Characters**: The function handles mixed whitespace characters (spaces, tabs, and carriage returns) simultaneously, allowing for robust parsing regardless of the formatting style used in the source code.

## Interactions with Other Components
- **Lexer Class State**: The `skipWhitespace()` function interacts with the Lexer class's internal state, particularly the `src` string containing the source code and the `pos` cursor indicating the current position within the string.
- **Tokenization Process**: By skipping whitespace, the function facilitates the subsequent tokenization process. It ensures that tokens are extracted based solely on their actual content rather than their surrounding formatting.
- **Efficiency**: Consuming whitespace efficiently is crucial for performance, especially when dealing with large source files. The use of a single loop and conditional checks minimizes overhead and maximizes speed.

Here is the implementation of the `skipWhitespace()` function:

```cpp
{
    while (pos < src.size() && (current() == ' ' || current() == '\t' || current() == '\r'))
        advance();
}
```

### Explanation of the Code
- **Loop Condition**: The `while` loop continues as long as `pos` is less than the size of the `src` string and the character at the current position (`current()`) is either a space, tab, or carriage return.
- **Character Check**: Inside the loop, the `current()` method retrieves the character at the current position `pos`.
- **Advance Cursor**: If the character is a whitespace character, the `advance()` method increments the `pos` cursor, effectively moving to the next character in the source code.
- **Edge Handling**: When a non-whitespace character is encountered or the end of the source code is reached, the loop terminates, ensuring that only relevant characters are considered for tokenization.

In summary, the `skipWhitespace()` function plays a critical role in the Quantum Language compiler's Lexer by efficiently removing unnecessary whitespace characters, thus simplifying the tokenization process and improving overall performance.