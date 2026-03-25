# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are represented using curly braces `{}` and can contain pairs of keys and values. This function handles both standard dictionary entries and shorthand properties.

## Parameters and Return Value

- **Parameters**: None explicitly listed in the provided snippet, but it relies on global state such as `current()` and `pos`.
- **Return Value**: An instance of `DictLiteral`, which represents the parsed dictionary literal.

## How It Works

### Step-by-Step Parsing Process

1. **Initialization**:
   - The function starts by recording the current line number (`ln`) where the dictionary literal begins.
   - It then checks if the next token is an opening brace `{`. If not, it throws an error indicating that a brace was expected.

2. **Skipping Newlines**:
   - After expecting the opening brace, the function skips any newline characters to ensure proper parsing of the dictionary content.

3. **Parsing the Dictionary Body**:
   - A `while` loop is used to iterate through the tokens until either a closing brace `}` is encountered or the end of the input is reached.
   - Inside the loop, there are two main branches to handle different scenarios:
     - **Spread Syntax**: If the current token is an identifier followed by "...", it indicates a spread syntax (`{...obj}`). The function consumes the "...", parses the expression following it, and adds a pair to the dictionary with a `nullptr` key. This special sentinel is used to indicate that the pair is a spread.
     - **Standard Key-Value Pairs**: For standard key-value pairs, the function checks if the current token is an identifier, a type keyword, or a string literal. If these conditions are met, it attempts to determine if the next token is a colon `:`:
       - If the next token is a colon, it treats the current token as a bare string key and converts it into a `StringLiteral` node.
       - If the next token is a comma `,` or a closing brace `}`, it treats the current token as a shorthand property. In this case, it also converts the token into a `StringLiteral` node but sets a flag (`isShorthand`) to indicate that this is a shorthand property.
       - If neither condition is met, it calls `parseExp` to parse the key expression.

4. **Handling Commas and Closing Braces**:
   - After processing each key-value pair, the function checks if the next token is a comma `,`. If so, it skips any newlines and continues to the next iteration.
   - If the next token is a closing brace `}`, the loop breaks, indicating the end of the dictionary literal.

5. **Returning the Result**:
   - Once the loop completes, the function returns the parsed `DictLiteral`.

## Edge Cases

- **Empty Dictionary Literal**: If the dictionary literal is empty (i.e., only contains an opening brace followed by a closing brace), the function will correctly return an empty `DictLiteral`.
- **Invalid Spread Syntax**: If the spread syntax is used incorrectly (e.g., without a preceding object), the function will throw an appropriate error.
- **Misplaced Comma**: If a comma is placed after a closing brace, the function will skip the comma and continue parsing, potentially leading to unexpected behavior.

## Interactions with Other Components

- **Token Stream**: The function uses the global token stream (`tokens`) and its position (`pos`) to read and consume tokens.
- **Error Handling**: If the expected tokens are not found during parsing, the function throws errors using mechanisms like `expect` and `consume`.
- **AST Construction**: The function constructs an Abstract Syntax Tree (AST) by creating nodes such as `StringLiteral` and adding them to the `dict.pairs`.
- **Utility Functions**: The function utilizes utility functions like `skipNewlines` and `match` to manage whitespace and token matching efficiently.

This comprehensive approach ensures that the `parseDictLiteral` function accurately parses dictionary literals according to the Quantum Language's grammar rules, handling various edge cases and interacting seamlessly with other parts of the compiler.