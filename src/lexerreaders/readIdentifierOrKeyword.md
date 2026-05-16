# `readIdentifierOrKeyword`

The `readIdentifierOrKeyword` function is designed to identify and process identifiers and keywords in the source code of the Quantum Language being compiled. This function plays a crucial role in the lexer phase of the compiler, where it converts raw text into meaningful tokens that can be further processed by the parser.

## What it Does

The primary purpose of `readIdentifierOrKeyword` is to recognize and categorize words in the source code as either identifiers or keywords. An identifier typically refers to a variable name, function name, or any other user-defined name, whereas a keyword is a reserved word with special significance in the language syntax.

## Why it Works This Way

This function operates by iterating through the characters in the source code starting from the current position (`pos`). It checks if each character is alphanumeric or an underscore, which are valid characters for identifiers. If such characters are found, they are appended to a temporary string (`id`). Once a non-alphanumeric or non-underscore character is encountered, the function stops reading and processes the identified token.

Additionally, the function handles two specific types of strings:
1. **Raw Strings**: These are prefixed with 'r' or 'R', followed by a quotation mark ('"' or "'"). The function reads characters until it encounters the matching quotation mark, skipping any escape sequences.
2. **Formatted Strings (f-strings)**: These are prefixed with 'f' or 'F', followed by a quotation mark. The function reads characters until it encounters the matching quotation mark. Inside formatted strings, curly braces `{}` are treated specially to denote expressions that should be evaluated at runtime.

## Parameters/Return Value

### Parameters
- None directly specified in the provided snippet; however, implicit parameters include:
  - `src`: A reference to the source code string being lexed.
  - `pos`: The current position in the source code string.
  - `line`: The current line number in the source code.
  - `col`: The current column number in the source code.

### Return Value
- Returns a `Token` object representing the identified identifier, keyword, or string. The type of token depends on whether the input was an identifier, keyword, or one of the handled string types.

## Edge Cases

- **Empty Identifier**: If the first character is not alphanumeric or an underscore, the function returns an empty identifier token.
- **String Literal**: If the input starts with a quotation mark but does not match the expected format for a raw or formatted string, the function treats it as a regular string literal.
- **End of Source Code**: If the end of the source code is reached before identifying a complete token, the function returns the partially identified token up to that point.

## Interactions with Other Components

- **Lexer Class**: `readIdentifierOrKeyword` is part of the Lexer class, which is responsible for breaking down the source code into individual tokens.
- **Parser Class**: Tokens produced by `readIdentifierOrKeyword` are consumed by the Parser class to construct the abstract syntax tree (AST) of the program.
- **Error Handling**: While not explicitly shown in the snippet, this function likely interacts with error handling mechanisms to report issues such as unterminated strings or invalid characters within identifiers.

In summary, `readIdentifierOrKeyword` is essential for accurately parsing identifiers and keywords in the Quantum Language, ensuring that subsequent phases of compilation can proceed correctly. Its ability to handle different types of strings adds flexibility to the language's syntax, making it more powerful and expressive.