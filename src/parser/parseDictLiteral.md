# `parseDictLiteral`

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are represented as `{}` and can contain pairs of keys and values, as well as shorthand properties. This function ensures that the syntax of dictionary literals is correctly interpreted and constructs an abstract syntax tree (AST) node representing the parsed dictionary literal.

## Parameters

- None

## Return Value

- An `ASTNodePtr` object representing the parsed dictionary literal.

## How It Works

The function starts by checking the current token to ensure it is an opening brace (`{`). If not, it throws an error indicating that a dictionary literal must start with an opening brace. After confirming the opening brace, it skips any new lines and initializes an empty `DictLiteral` object.

The function then enters a loop that continues until it encounters either a closing brace (`}`) or reaches the end of the input. Within the loop, it checks for the spread operator (`...`), which allows one dictionary to be merged into another. If the spread operator is found, it consumes the token and parses the expression following it using the `parseUnary()` function. The resulting expression is added to the dictionary's pairs list with a `nullptr` key, serving as a sentinel to indicate that this pair represents a spread operation.

If the spread operator is not found, the function proceeds to parse the key of the dictionary pair. Keys can be:
1. A quoted string literal.
2. A numeric literal.
3. A bare identifier.
4. A type keyword.

For each potential key type, the function first checks if the next token is a colon (`:`). If so, it treats the current token as a bare string key and creates a corresponding `StringLiteral` node. If the next token is a comma (`,`) or a closing brace (`}`), it assumes the current token is part of a shorthand property, where the key name is used both as the key and the value. In this case, it also creates a `StringLiteral` node but sets the `isShorthand` flag to `true`.

After parsing the key, the function expects a colon (`:`) followed by the value of the dictionary pair. The value is parsed using the `parseExp()` function, which handles various expressions including literals, unary operations, binary operations, etc.

Once the key-value pair is successfully parsed, it is added to the `dict.pairs` list. The function then skips any new lines and checks if the next token is a comma (`,`). If it is, the loop continues; otherwise, the loop breaks.

Finally, when the loop ends due to encountering a closing brace or reaching the end of the input, the function returns an `ASTNodePtr` object containing the parsed `DictLiteral`. This node can be further processed by other parts of the compiler to generate executable quantum code.

## Edge Cases

- **Empty Dictionary Literal**: If the dictionary literal is empty (`{}`), the function will still return an `ASTNodePtr` object representing an empty dictionary.
- **Invalid Syntax**: If the dictionary literal contains invalid syntax, such as missing colons between key-value pairs or misplaced commas, the function will throw appropriate errors.
- **Spread Operator Usage**: The function correctly handles the spread operator, allowing one dictionary to be merged into another during compilation.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens representing the source code. It uses functions like `current()`, `consume()`, and `pos` to interact with the tokenizer.
- **Error Handling**: If the function encounters unexpected tokens or syntax errors, it calls `expect()` or `match()` to report appropriate errors and halt parsing.
- **Abstract Syntax Tree Construction**: Once the dictionary literal is fully parsed, the function constructs an `ASTNodePtr` object containing the parsed `DictLiteral` and returns it for further processing by other components of the compiler.