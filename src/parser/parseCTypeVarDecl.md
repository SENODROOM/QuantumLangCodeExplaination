# parseCTypeVarDecl

The `parseCTypeVarDecl` function in the Quantum Language compiler is designed to parse variable declarations of C-like types. This includes handling various modifiers such as pointers (`*`) and const qualifiers (`const`). The function aims to extract the type information and the variable name from the input tokens.

## What It Does

- **Extracts Type Information**: The function identifies whether the declared variable is a pointer and handles any const qualifiers that may precede the type or the pointer.
- **Handles Function Pointers**: It also recognizes and parses function pointers, which can include additional syntax for member functions.
- **Accepts Variable Names**: Finally, it consumes the token representing the variable name, ensuring that it matches expected types like identifiers or keywords used as variable names.

## Why It Works This Way

### Pointer Handling

- **Loop Through Modifiers**: The function uses a loop to consume all adjacent pointer stars (`*`) and const qualifiers (`const`). This ensures that complex declarations like `int** const p` are correctly parsed.
- **Track Pointer Status**: By setting the `isPointer` flag within the loop, the function keeps track of whether the variable being declared is a pointer.

### Function Pointers

- **Check for Parentheses**: If an opening parenthesis (`(`) is encountered, the function assumes it might be part of a function pointer declaration.
- **Nested Loop for Modifiers**: Similar to the main loop, another nested loop is used to handle any additional pointer stars or const qualifiers inside the parentheses.
- **Discard Class Name**: For member function pointers, the class name before the scope resolution operator (`::*`) is consumed but discarded. Only the variable name is retained.

### Variable Name Parsing

- **Identifier Check**: The function first checks if the next token is an identifier. If so, it consumes it as the variable name.
- **Keyword Check**: If not an identifier, it checks if the token is either `INPUT` or `PRINT`, which are special keywords in the Quantum Language. These are also consumed as valid variable names.
- **Expectation Handling**: If neither an identifier nor a keyword is found, the function throws an error indicating that a valid variable name was expected after the type.

## Parameters/Return Value

- **Parameters**: None explicitly mentioned in the code snippet provided.
- **Return Value**: The function returns a `Token` object representing the variable name that was successfully parsed.

## Edge Cases

- **Empty Declaration**: If there are no tokens left to parse or the tokens do not form a valid variable declaration, the function will throw an error.
- **Invalid Keywords**: Using invalid keywords like `INPUT` or `PRINT` where they are not expected will result in an error.
- **Complex Types**: Declarations involving complex types or multiple levels of indirection (like `int***`) are handled gracefully without errors.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide a stream of tokens. Each token represents a part of the source code.
- **Error Handling**: Errors during parsing are managed through calls to `expect`, which throws exceptions when a token of the expected type is not found.
- **Scope Resolution**: For member function pointers, the class name before the scope resolution operator is ignored, demonstrating how the function interacts with other parts of the compiler that manage scope and symbol tables.

This function is crucial for accurately interpreting the type and name of variables in C-like declarations, ensuring that subsequent stages of compilation can proceed with correct type information.