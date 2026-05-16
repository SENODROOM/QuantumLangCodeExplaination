# parseCTypeVarDecl

## Overview

The `parseCTypeVarDecl` function in the Quantum Language compiler is designed to parse variable declarations that adhere to C-like syntax rules. This includes handling declarations of variables that may involve pointers, const qualifiers, and member function pointers. The function ensures accurate parsing by managing various token sequences and correctly identifying the type, pointer status, and name of the variable.

## Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet, but implicit parameters include the current state of the parser (`current()`), a list of tokens (`tokens`), and the position within the token list (`pos`).
- **Return Value**: The function returns a parsed variable declaration, which typically involves setting up data structures to represent the variable's type, whether it is a pointer, and its name.

## How It Works

The function operates through several key steps:

1. **Initialization**:
   - It starts by recording the current line number (`ln`) using `current().line`.

2. **Parsing Pointer Stars and Const Qualifiers**:
   - The function checks for any `STAR` (pointer) or `CONST` tokens following the type specifier. These tokens can appear before or after the variable name, and they indicate whether the variable is a pointer and/or if it is constant.
   - If a `STAR` token is found, it sets `isPointer` to `true`.
   - If a `CONST` token is found, it continues to process subsequent tokens until a non-pointer/non-const token is encountered.

3. **Handling Function Pointers**:
   - The function also checks for an optional opening parenthesis `LPAREN`, which might indicate a function pointer declaration.
   - If an opening parenthesis is found, it consumes the token and then processes any additional `STAR` or `CONST` tokens similarly to the previous step.

4. **Identifying Member Function Pointers**:
   - For member function pointers, the function looks ahead two tokens to identify a pattern where an identifier followed by two colons (`::`) indicates a class scope.
   - Upon detecting this pattern, it consumes the class name and the two colons, effectively discarding them since they are not part of the variable name itself.
   - It then continues to check for `STAR` tokens to determine if the member function pointer is a pointer.

5. **Accepting Variable Names**:
   - Finally, the function accepts either an `IDENTIFIER` token or a keyword token (`INPUT` or `PRINT`) as the variable name.
   - If an `IDENTIFIER` token is found, it consumes the token and assigns it to `nameToken`.
   - If a keyword token is found, it consumes the token and assigns it to `nameToken`. However, if neither token is found, it raises an error indicating that a variable name was expected after the type.

## Edge Cases

- **Empty Declaration**: If there are no tokens left to parse or if the next token is not an `IDENTIFIER`, the function will raise an error.
- **Misplaced Tokens**: If tokens such as `STAR` or `CONST` are misplaced (e.g., appearing before the type specifier), the function will continue processing until it encounters a valid token sequence.
- **Complex Types**: While the function handles basic types and pointers, more complex types or nested pointers might require additional logic to ensure correct parsing.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses error-handling mechanisms to manage unexpected token sequences or missing identifiers.
- **Symbol Table**: Once a variable declaration is successfully parsed, it may interact with the symbol table to store information about the variable, including its type, pointer status, and name.

Overall, `parseCTypeVarDecl` plays a crucial role in accurately parsing variable declarations in the Quantum Language compiler, ensuring that the subsequent stages of compilation have the necessary information to proceed correctly.