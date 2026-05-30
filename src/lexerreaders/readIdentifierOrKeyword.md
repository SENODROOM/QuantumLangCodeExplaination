# `readIdentifierOrKeyword`

## Overview

The `readIdentifierOrKeyword` function is a crucial part of the lexer phase in the Quantum Language compiler. Its primary responsibility is to identify and process both identifiers and keywords within the source code. This function ensures that tokens are correctly categorized based on their lexical structure, which is essential for subsequent phases of parsing and compilation.

## Parameters

- None

## Return Value

- Returns a `Token` object representing either an identifier or a keyword.

## How It Works

The function starts by reading characters from the input source until it encounters a character that is not alphanumeric or an underscore. These characters form the initial identifier or keyword.

If the identifier happens to be `"r"` or `"R"` followed by a double or single quote (`"`, `'`), it identifies this as a **raw string**. The function reads all characters up to the matching closing quote, ignoring any escape sequences. Once the closing quote is found, the function returns a `Token` of type `STRING`.

Similarly, if the identifier is `"f"` or `"F"` followed by a double or single quote, it identifies this as an **f-string**, also known as a formatted string literal. F-strings allow embedded expressions within string literals, similar to Python's f-strings. The function reads characters up to the closing quote, converting `{}` syntax to `${}` before returning a `Token` of type `TEMPLATE_STRING`.

For regular identifiers and keywords, the function simply constructs a string from the read characters and checks against a predefined list of keywords. If the constructed string matches a keyword, it returns a `Token` of type `KEYWORD`. Otherwise, it returns a `Token` of type `IDENTIFIER`.

## Edge Cases

- **Empty String**: If the input stream is empty, the function will return an empty token.
- **Invalid Identifier**: If the first character is not alphanumeric or an underscore, the function will return an empty token.
- **Unclosed Quotes**: If a raw or f-string is started but never closed, the function will continue reading until the end of the file and return an incomplete token.
- **Nested Expressions**: In f-strings, nested curly braces can cause issues. The function handles these by tracking the depth of curly braces and ensuring that they are properly matched.

## Interactions With Other Components

- **Lexer Class**: This function is part of the Lexer class, which is responsible for breaking down the source code into individual tokens.
- **Token Class**: Tokens are created using the `Token` constructor, which takes parameters such as token type, value, and position information.
- **Predefined Keywords List**: The function uses a predefined list of keywords to check if the constructed identifier matches any of them.

By accurately identifying and categorizing identifiers and keywords, the `readIdentifierOrKeyword` function plays a vital role in the lexer phase of the Quantum Language compiler, providing a solid foundation for further parsing and semantic analysis.