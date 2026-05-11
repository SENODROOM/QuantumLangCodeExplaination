# `readIdentifierOrKeyword`

The `readIdentifierOrKeyword` function is designed to identify and process identifiers and keywords in the source code of the Quantum Language being compiled. This function plays a crucial role in the lexer phase of the compiler, where it converts raw text into meaningful tokens that can be further processed by the parser.

## What It Does

The function reads characters from the input source code until it encounters a non-alphanumeric character or an underscore (`_`). If the sequence of characters forms a valid identifier or keyword, it returns a corresponding token. Additionally, it handles special prefixes for raw strings (`r"..."`, `r'...'`) and formatted string literals (`f"..."`, `f'...'`).

## Why It Works This Way

This approach ensures that all valid identifiers and keywords are correctly recognized and classified. By iterating through the characters and checking their type, the function can differentiate between identifiers, keywords, and special string literals. The handling of raw and formatted string literals allows for accurate parsing of these constructs, which may contain embedded expressions or escape sequences.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet, but it likely takes the following implicit parameters:
    - `src`: A reference to the source code string being lexed.
    - `pos`: A reference to the current position in the source code string.
    - `line`: The current line number in the source code.
    - `col`: The current column number in the source code.

- **Return Value**:
  - Returns a `Token` object representing the identified identifier, keyword, or string literal. The token includes its type, value, starting line, and starting column.

## Edge Cases

- **Empty Identifier**: If the first character encountered is not alphanumeric or an underscore, the function should return a token indicating the end of an identifier or keyword.
- **Raw String Literal**: If the function encounters a raw string literal prefix (`r"..."`, `r'...'`) followed by a matching quote, it should correctly handle the literal without interpreting escape sequences.
- **Formatted String Literal**: If the function encounters a formatted string literal prefix (`f"..."`, `f'...'`) followed by a matching quote, it should convert `{expr}` parts to `${expr}` before re-lexing as a template literal.

## Interactions With Other Components

- **Lexer Phase**: This function is integral to the lexer phase, converting raw text into tokens that represent the structure of the source code.
- **Parser Phase**: Tokens returned by `readIdentifierOrKeyword` are used by the parser to construct the abstract syntax tree (AST) of the program.
- **Error Handling**: The function implicitly interacts with error handling mechanisms within the compiler to report invalid identifiers or syntax errors related to string literals.

Overall, the `readIdentifierOrKeyword` function is essential for accurately parsing identifiers, keywords, and special string literals in the Quantum Language source code, ensuring that the subsequent phases of the compiler can work with well-formed tokens.