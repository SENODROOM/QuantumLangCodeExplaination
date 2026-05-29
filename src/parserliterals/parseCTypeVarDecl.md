# parseCTypeVarDecl

## Overview

The `parseCTypeVarDecl` function in the Quantum Language compiler is designed to parse variable declarations that adhere to C-like syntax rules. This includes handling declarations of variables that may involve pointers, const qualifiers, and member function pointers.

## Functionality

The function processes tokens to determine the type and declaration details of a variable. It handles various scenarios such as:

- Variables declared with pointer types (`int*`, `const int*`, etc.)
- Member function pointers (`void (Class::*)()`)

### Parsing Pointer Types

The function first checks for any `STAR` (pointer) or `CONST` (constant qualifier) tokens between the type and the variable name. It iterates through these tokens to set the `isPointer` flag accordingly.

```cpp
bool isPointer = false;
while (check(TokenType::STAR) || check(TokenType::CONST))
{
    if (check(TokenType::STAR))
        isPointer = true;
    consume();
}
```

### Handling Function Pointers

If the token sequence indicates a potential function pointer (e.g., `void (*fp)`), the function consumes the `LPAREN` token and continues checking for pointer or constant qualifiers within the parentheses.

```cpp
bool hasParen = false;
if (check(TokenType::LPAREN))
{
    hasParen = true;
    consume();
    while (check(TokenType::STAR) || check(TokenType::CONST))
    {
        if (check(TokenType::STAR))
            isPointer = true;
        consume();
    }
}
```

### Processing Class Member Pointers

For class member pointers (e.g., `void (Class::*)()`), the function checks for three consecutive colons (`:::`). If found, it consumes the class name and the two colons, then discards them. The function also sets the `isPointer` flag if there are any pointer qualifiers following the class name.

```cpp
std::string prefix = "";
if (check(TokenType::IDENTIFIER))
{
    size_t la = pos + 1;
    if (la < tokens.size() && tokens[la].type == TokenType::COLON)
    {
        size_t la2 = la + 1;
        if (la2 < tokens.size() && tokens[la2].type == TokenType::COLON)
        {
            std::string className = consume().value; // eat ClassName (discard for naming)
            consume();                               // colon
            consume();                               // colon
            // prefix is discarded — variable gets just its short name
            while (check(TokenType::STAR))
            {
                isPointer = true;
                consume();
            }
        }
    }
}
```

### Variable Name Consumption

Finally, the function accepts either an `IDENTIFIER` token or specific keywords (`INPUT`, `PRINT`) as valid variable names. If neither is found, it raises an error indicating that a variable name was expected after the type.

```cpp
Token nameToken = check(TokenType::IDENTIFIER) ? consume() : (check(TokenType::INPUT) || check(TokenType::PRINT)) ? consume()
                                                                                                                      : expect(TokenType::IDENTIFIER, "Expected variable name after type");
```

## Parameters and Return Value

- **Parameters**: None explicitly listed in the provided code snippet.
- **Return Value**: A `Token` representing the parsed variable name.

## Edge Cases

- **Empty Declaration**: If the input sequence does not contain a valid variable name, the function will raise an error.
- **Misplaced Qualifiers**: The function assumes that pointer and const qualifiers are correctly placed between the type and the variable name. Misplacement will lead to incorrect parsing.
- **Invalid Syntax**: Any deviation from the expected C-like syntax will result in errors during parsing.

## Interactions with Other Components

The `parseCTypeVarDecl` function interacts with several other components within the compiler:

- **Tokenizer**: Consumes tokens to identify the type, pointer qualifiers, and variable name.
- **Error Handler**: Raises errors when encountering unexpected tokens or malformed syntax.
- **Symbol Table**: Used implicitly to store information about the parsed variable, including its type and whether it's a pointer.

This function is crucial for accurately interpreting variable declarations in the Quantum Language source code, ensuring that subsequent phases of compilation can proceed without errors.