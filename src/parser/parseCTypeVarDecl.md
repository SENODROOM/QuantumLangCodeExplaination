## parseCTypeVarDecl

### Overview

The `parseCTypeVarDecl` function in the Quantum Language compiler is designed to parse variable declarations of C-like types. It handles various modifiers such as pointers (`*`) and const qualifiers (`const`). The primary goal is to extract the type information and the variable name from the input tokens.

### Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet, but it operates on global state within the parser class, including `current()` and `tokens`.
- **Return Type**: The function returns a `std::pair<TypeInfo, Token>`, where `TypeInfo` contains details about the parsed type, and `Token` represents the variable name.

### How It Works

1. **Initialization**:
   - The function starts by retrieving the current line number using `current().line`.

2. **Handling Pointers and Const Qualifiers**:
   - It initializes a boolean flag `isPointer` to `false`. The function then enters a loop that consumes any `TokenType::STAR` (pointer) and `TokenType::CONST` (const qualifier) tokens found between the type and the variable name. If a `TokenType::STAR` is encountered, `isPointer` is set to `true`.

3. **Checking for Function Pointer Syntax**:
   - The function checks if there is an opening parenthesis `(`, which might indicate a function pointer syntax. If found, it sets `hasParen` to `true` and consumes the token. It then enters another loop to handle any additional pointer or const qualifier tokens inside the parentheses.

4. **Handling Member Pointers**:
   - The function checks if the next two tokens form a member pointer syntax (`:::`). If so, it consumes the class name, followed by two colons. The class name is ignored, and the function continues to consume any remaining pointer or const qualifier tokens.

5. **Accepting Variable Names**:
   - Finally, the function accepts either an `IDENTIFIER` token representing the variable name or specific keyword tokens (`INPUT` or `PRINT`) used as variable or parameter names. If neither is found, it throws an error indicating that a variable name was expected after the type.

### Edge Cases

- **Empty Declaration**: If the input tokens do not contain a valid variable declaration, the function will throw an error.
- **Invalid Modifiers**: If the input contains invalid modifiers (e.g., multiple consecutive `const` qualifiers without a preceding type), the function will throw an error.
- **Member Pointers Without Class Name**: If the member pointer syntax is used without a preceding class name, the function will ignore the class name but still consume any subsequent pointer or const qualifier tokens.

### Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: Errors during parsing are handled through the `expect` method, which ensures that the correct token type is consumed at each step.
- **Symbol Table**: While not directly shown in the provided code snippet, the parsed type and variable name would typically be added to a symbol table for further use in semantic analysis and code generation phases.

This function is crucial for accurately interpreting variable declarations in the Quantum Language, ensuring that types and modifiers are correctly parsed and can be utilized in subsequent stages of compilation.