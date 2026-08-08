# `readIdentifierOrKeyword`

## Overview

The `readIdentifierOrKeyword` function is a critical component of the lexer phase in the Quantum Language compiler. It identifies and processes both identifiers and keywords within the source code. The function ensures that tokens are correctly classified based on their content and context.

## Functionality

This function reads characters from the input source code until it encounters a non-alphanumeric character or an underscore. If the sequence of characters forms a valid identifier or keyword, it returns a corresponding token. Additionally, it handles special prefixes for raw strings (`r"..."`, `r'...'`) and formatted string literals (`f"..."`, `f'...'`). For raw strings, it skips the opening quote and continues reading until the closing quote is found, returning a `STRING` token. For formatted string literals, it converts `{}` expressions into `${}` format and then re-lexes them as template literals, returning a `TEMPLATE` token.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined; operates directly on global state variables such as `src`, `pos`, `line`, and `col`.

- **Return Value**:
  - Returns a `Token` object representing either an `IDENTIFIER`, `KEYWORD`, `STRING`, or `TEMPLATE`. If the input does not form a valid token, it may return an `UNKNOWN` token.

## Edge Cases

- **Empty Input**: If the input source code is empty or reaches its end before forming a valid token, the function should handle gracefully and return an appropriate token.
- **Invalid Identifiers/Keywords**: If the sequence of characters does not match any known identifier or keyword, the function should return an `UNKNOWN` token.
- **Raw String Prefix**: The function must correctly identify and handle raw string prefixes without interpreting escape sequences.
- **Formatted String Literals**: The function needs to accurately convert `{}` expressions into `${}` format and re-lex them appropriately.

## Interactions with Other Components

- **Lexer State Management**: This function interacts with the lexer's internal state variables (`src`, `pos`, `line`, `col`) to track the position and content of the source code being processed.
- **Token Classification**: It classifies tokens based on patterns recognized during the reading process, interacting with the `TokenType` enumeration to determine the type of each token.
- **Error Handling**: While not explicitly shown in the provided snippet, the function likely includes error handling mechanisms to manage unexpected inputs or malformed tokens, potentially interacting with higher-level components for reporting errors.

Overall, `readIdentifierOrKeyword` plays a vital role in the initial parsing stage of the Quantum Language compiler, ensuring that basic elements of the language are correctly identified and categorized.