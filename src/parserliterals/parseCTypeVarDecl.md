# parseCTypeVarDecl

## Overview

The `parseCTypeVarDecl` function in the Quantum Language compiler is designed to parse variable declarations that adhere to C-like syntax rules. This includes handling declarations of variables that may involve pointers, const qualifiers, and member function pointers. The function ensures that the correct syntax is followed and extracts relevant information about the variable declaration.

## Parameters/Return Value

### Parameters
- None

### Return Value
- `std::optional<Token>`: Returns an `std::optional<Token>` containing the parsed variable declaration token if successful, or `std::nullopt` if an error occurs during parsing.

## How It Works

The function operates by iterating through the tokens and identifying the type of variable declaration being parsed. It checks for various types of syntax elements such as pointer stars (`*`), const qualifiers (`const`), and special keywords like `input` and `print`.

1. **Initialization**:
   - The function starts by recording the current line number using `current().line`.

2. **Pointer and Const Qualifiers**:
   - It then enters a loop to consume any pointer stars (`*`) and const qualifiers (`const`) found between the type and the variable name. These can appear in different combinations, such as `int* p`, `int *p`, `int* const p`, or `const int* const p`.
   - If a pointer star is encountered, `isPointer` is set to `true`. Each token is consumed until all pointer stars and const qualifiers have been processed.

3. **Optional Parentheses for Function Pointers**:
   - The function also checks for the presence of optional parentheses `()` which might indicate a function pointer declaration. If parentheses are found, they are consumed along with any additional pointer stars or const qualifiers within them.
   - After consuming the parentheses, the function continues to process any remaining pointer stars or const qualifiers.

4. **Member Function Pointer Handling**:
   - The function then checks if the next token sequence indicates a member function pointer declaration. This is done by looking for two consecutive colons (`::`). If found, the class name is consumed, but it is immediately discarded since the variable name itself will not include the class name.
   - The function then processes any remaining pointer stars or const qualifiers.

5. **Variable Name Parsing**:
   - Finally, the function accepts either an identifier token representing the variable name or specific keyword tokens like `input` and `print`. If neither is found, an error is thrown indicating that a variable name was expected after the type.

## Edge Cases

- **Empty Declaration**: If there are no tokens left to parse or the tokens do not form a valid variable declaration, the function returns `std::nullopt`.
- **Invalid Syntax**: If the syntax does not match the expected patterns (e.g., missing identifier after type), the function throws an error.
- **Complex Types**: The function can handle complex types such as arrays, structures, and unions by extending the logic to recognize these constructs.

## Interactions with Other Components

- **Tokenizer**: The `parseCTypeVarDecl` function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handler**: Errors encountered during parsing are handled by the error handler component, ensuring that the compiler provides meaningful feedback to the user.
- **Symbol Table**: Once a valid variable declaration is parsed, the function updates the symbol table with the necessary information about the variable, including its type, whether it is a pointer, and whether it is const.

This comprehensive approach allows the `parseCTypeVarDecl` function to accurately parse a wide range of variable declarations in the Quantum Language, ensuring that the compiler can generate appropriate code based on the parsed syntax.