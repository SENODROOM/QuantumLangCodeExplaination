# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is designed to parse dictionary literals from the source code. Dictionary literals are structured using curly braces `{}`, containing pairs of keys and values. This function is crucial for handling data structures that require key-value mappings in the language.

### Why It Works This Way

The implementation of `parseDictLiteral` follows a systematic approach to ensure accurate parsing of dictionary literals:

1. **Initialization**: The function begins by recording the line number where the dictionary literal starts (`ln`). It then expects a left brace `{`, indicating the start of the dictionary, and skips any new lines before proceeding.

2. **Parsing Pairs**: Inside a loop, the function continuously parses key-value pairs until a right brace `}` is encountered or the end of the file is reached. Each pair consists of a key followed by a colon `:` and a value.

3. **Handling Spreads**: The function checks for the spread operator `...`. If found, it consumes the operator and recursively parses an expression, treating it as a spread expression. A special sentinel (`nullptr`) is used to indicate that the key is a spread expression.

4. **Key Parsing**: Keys can be either quoted strings, numbers, bare identifiers, or type keywords. The function handles each case appropriately:
   - **Bare Identifiers**: If the next token is a colon `:`, the current identifier is treated as a bare string key.
   - **Shorthand Properties**: If the next token is a comma `,` or a right brace `}`, the current identifier is treated as a shorthand property, where the value is implicitly taken from the same variable name.

5. **Value Parsing**: After parsing the key, the function calls `parseExp()` to parse the value associated with the key.

6. **Edge Cases**:
   - The function gracefully handles missing colons between keys and values.
   - It correctly processes nested dictionaries and arrays within dictionary literals.
   - The presence of comments or new lines is managed to maintain the integrity of the parsed structure.

7. **Interactions with Other Components**:
   - The function interacts with the `Tokenizer` to retrieve the next token and determine its type.
   - It uses the `ErrorReporter` to report syntax errors if unexpected tokens are encountered.
   - The parsed AST nodes are constructed and added to the `dict.pairs` vector, which represents the dictionary literal in the abstract syntax tree.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed; however, it relies on global state such as the current position in the token stream (`pos`), the list of tokens (`tokens`), and the tokenizer functions (`current()`, `consume()`, etc.).

- **Return Value**:
  - Returns a `DictLiteral` object representing the parsed dictionary literal. The `DictLiteral` contains a vector of pairs, where each pair consists of a key and a value. If a spread expression is encountered, the key is set to `nullptr`.

## Example Usage

Here's an example of how `parseDictLiteral` might be called and used in the context of the Quantum Language compiler:

```cpp
// Assuming 'tokenizer' has been initialized and positioned at the start of the dictionary literal
auto dictLiteral = parseDictLiteral();

// Now 'dictLiteral' contains the parsed dictionary literal, which can be further processed or analyzed
for (const auto& pair : dictLiteral.pairs)
{
    if (pair.first != nullptr)
    {
        // Process regular key-value pair
        std::cout << "Key: " << pair.first->toString() << ", Value: " << pair.second->toString() << std::endl;
    }
    else
    {
        // Handle spread expression
        std::cout << "Spread Expression: " << pair.second->toString() << std::endl;
    }
}
```

This example demonstrates how the parsed dictionary literal can be iterated over to access individual key-value pairs or spread expressions.