# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is designed to parse dictionary literals from the source code. Dictionary literals are represented using curly braces `{}` and consist of key-value pairs. This function ensures that the dictionary literals are parsed accurately and correctly handles various scenarios, including nested dictionaries and spread syntax.

## Parameters

- None

## Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed dictionary literal.
- If parsing fails due to unexpected token types or syntax errors, the function throws an exception.

## Edge Cases

- **Empty Dictionary**: An empty dictionary literal `{}` is handled gracefully, returning an empty dictionary node.
- **Nested Dictionaries**: The function can handle nested dictionary literals like `{{"key1": "val1"}, {"key2": "val2"}}`. It distinguishes between these and array literals based on the context.
- **Spread Syntax**: The function supports the spread syntax `...`, allowing for the merging of one dictionary into another during parsing. For example, `{"name": "Alice", ...otherDict}`.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens (`tokens`) which it processes to build the AST.
- **Error Handling**: If the parser encounters unexpected tokens or syntax errors, it uses error handling mechanisms provided by the compiler to report issues.
- **Expression Parsing**: When encountering values in the dictionary, the function calls `parseExpr()` to parse those expressions into their corresponding AST nodes.

## Implementation Details

### Line Number Tracking

The function starts by tracking the line number where the dictionary literal begins using `current().line`.

### Brace Initialization List Check

To determine whether the literal should be parsed as a dictionary or an array, the function checks:
- If the next token is another opening brace `{`, indicating a nested initialization list, which should be treated as an array of arrays.
- If the next token is either a string or a number, followed by a comma `,` or a closing brace `}`, it also indicates that the literal should be parsed as an array rather than a dictionary.

### Parsing the Dictionary Literal

If the literal is determined to be a dictionary, the function enters a loop that continues until it encounters a closing brace `}` or reaches the end of the input:
- **Key Parsing**: The function accepts keys which can be:
  - A quoted string (`TokenType::STRING`)
  - A number (`TokenType::NUMBER`)
  - A bare identifier (`TokenType::IDENTIFIER`)
  - A type keyword (`isCTypeKeyword(current().type)`)

- **Value Parsing**: After parsing the key, the function parses the associated value using `parseExpr()` and adds the key-value pair to the dictionary.

- **Spread Syntax Handling**: If the spread syntax `...` is encountered, the function consumes it and parses the following expression as a unary expression. This expression represents the dictionary to be spread. The key for this spread entry is set to `nullptr` as a sentinel value to indicate that it's a spread operation.

### Error Handling

Throughout the parsing process, the function uses `expect()` and `consume()` methods to ensure correct token consumption and error reporting. If the expected token is not found, an appropriate error message is thrown.

### Example Usage

Here’s how you might call this function within the context of the compiler:

```cpp
// Assuming 'parser' is an instance of the Parser class
auto dictNode = parser.parseDictLiteral();
if (dictNode) {
    // Process the ASTNode representing the dictionary literal
} else {
    // Handle parsing failure
}
```

This function plays a crucial role in accurately interpreting dictionary literals in the Quantum Language source code, ensuring they are correctly transformed into the AST structure for further processing by the compiler.