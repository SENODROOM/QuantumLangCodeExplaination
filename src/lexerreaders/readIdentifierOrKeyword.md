# `readIdentifierOrKeyword`

The `readIdentifierOrKeyword` function is designed to identify and process identifiers and keywords in the source code of the Quantum Language being compiled. This function plays a crucial role in the lexer phase of the compiler, where it converts raw text into meaningful tokens that can be further processed by the parser.

## What It Does

The function reads characters from the input source code starting at the current position (`pos`) until it encounters a character that is not alphanumeric or an underscore. If the identifier starts with "r" or "R", followed by a double or single quote, it treats the enclosed string as a raw string, which means it will not interpret any escape sequences within the string. Similarly, if the identifier starts with "f" or "F", followed by a double or single quote, it treats the enclosed string as an f-string, which allows for embedded expressions that are evaluated during runtime and prefixed with `${}`.

## Why It Works This Way

This approach ensures that the lexer correctly identifies and categorizes different types of tokens:
- **Identifiers**: Words that do not match any predefined keyword but are valid for variable names, function names, etc.
- **Keywords**: Reserved words that have special meanings in the language, such as `let`, `if`, `else`, etc.
- **Raw Strings**: Literal strings without escape sequences, useful for paths, URLs, and other data that should not be interpreted.
- **F-Strings**: Template literals that allow for dynamic content insertion, enhancing readability and maintainability.

By handling these cases separately, the lexer can accurately parse the source code and generate the appropriate tokens for each part.

## Parameters/Return Value

### Parameters
- None

### Return Value
- Returns a `Token` object representing the identifier or keyword.
  - If the token is an identifier, its type is `TokenType::IDENTIFIER`.
  - If the token is a keyword, its type is `TokenType::KEYWORD`.
  - If the token is a raw string, its type is `TokenType::STRING`.
  - If the token is an f-string, its type is also `TokenType::STRING`.

## Edge Cases

- **Empty Identifier**: If the input starts with a non-alphanumeric character, the function returns an empty identifier token.
- **Invalid F-String Syntax**: If the f-string syntax is incorrect (e.g., missing closing brace), the function may enter an infinite loop or produce unexpected results.
- **Unicode Characters**: The function assumes that all characters are ASCII, so it does not handle Unicode characters specifically.

## Interactions With Other Components

- **Lexer Class**: This function is a member of the `Lexer` class, which manages the overall lexing process.
- **Parser Class**: The generated tokens are used by the parser to construct the abstract syntax tree (AST) of the program.
- **Error Handling**: If the function encounters an invalid token or syntax error, it should throw an exception or set an error flag that can be handled by the parser or higher-level components.

Overall, the `readIdentifierOrKeyword` function is essential for parsing the Quantum Language's syntax and ensuring that the subsequent stages of compilation can work with well-formed tokens.