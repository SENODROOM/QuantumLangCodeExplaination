# `parseParamList` Function

## Overview

The `parseParamList` function is essential for parsing parameter lists within the context of quantum language compilation. It ensures that the syntax of function declarations and definitions is accurately interpreted, identifying and processing each parameter correctly.

## Parameters

- None

## Return Value

- Returns a `std::vector<std::string>` containing the names of the parsed parameters.

## Edge Cases

1. **Empty Parameter List**: If the function encounters an empty parameter list (`()`), it will return an empty vector without throwing any errors.
2. **Syntax Errors**: The function checks for expected tokens such as `'('` and `')'`. If these tokens are missing or incorrectly placed, it will throw an error indicating the expected token.
3. **Template Arguments**: The function can handle template arguments within parameter types, such as `unique_ptr<int[]>`, `shared_ptr<Foo>`, etc. It skips over these arguments to identify the actual parameter name.
4. **Multi-Word Types**: The function can process multi-word types like `int x`, `Entity *m`, `Room &r`, etc., by consuming consecutive type keywords until a non-type keyword is encountered.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens representing the parameter list.
- **Error Handling**: The function uses error handling mechanisms provided by the compiler to report syntax errors when unexpected tokens are found.
- **Symbol Table**: After parsing the parameter list, the function may interact with the symbol table to register the parameters and their types.

## Detailed Explanation

### Syntax Parsing

The function begins by expecting a left parenthesis `'('` to mark the start of the parameter list. If this token is not found, it throws an error indicating the expected token.

```cpp
expect(TokenType::LPAREN, "Expected '('");
```

### Iterating Through Tokens

It then enters a loop that continues until it encounters a right parenthesis `')'` or reaches the end of the input tokens.

```cpp
while (!check(TokenType::RPAREN) && !atEnd())
{
```

#### Handling 'const' Modifier

Inside the loop, the function first checks if the current token is `'const'`. If so, it consumes the token to skip over the modifier.

```cpp
if (check(TokenType::CONST))
    consume(); // eat const
```

#### Processing C-Type Keywords

Next, the function checks if the current token is a C-type keyword (e.g., `int`, `char`). If it is, the function consumes the token and continues to consume subsequent tokens if they are also C-type keywords, allowing for multi-word types like `long long`.

```cpp
if (isCTypeKeyword(current().type))
{
    consume();
    while (isCTypeKeyword(current().type))
        consume(); // multi-word types
}
```

#### Identifying Parameter Names

If the current token is an identifier, the function identifies it as a potential parameter name. However, identifiers might be part of more complex types, such as those involving pointers (`*`) or references (`&`). To accurately identify the parameter name, the function performs a lookahead:

1. **Skipping Template Arguments**: If the next token is `<`, indicating the start of template arguments, the function skips over these arguments by incrementing a depth counter until it encounters `>` or `>>`.
   
   ```cpp
   if (la < tokens.size() && tokens[la].type == TokenType::LT)
   {
       int tdepth = 0;
       while (la < tokens.size())
       {
           if (tokens[la].type == TokenType::LT)
               tdepth++;
           else if (tokens[la].type == TokenType::GT)
           {
               tdepth--;
               la++;
               break;
           }
           else if (tokens[la].type == TokenType::RSHIFT)
           {
               tdepth -= 2;
               la++;
               break;
           }
           la++;
       }
   }
   ```

2. **Identifying Qualifiers**: The function then skips over any `*`, `&`, or `const` qualifiers following the identifier to ensure that only the actual parameter name is captured.

   ```cpp
   while (la < tokens.size() &&
          (tokens[la].type == TokenType::BIT_AND ||
           tokens[la].type == TokenType::STAR ||
           tokens[la].type == TokenType::CONST))
       la++;
   ```

### Registering Parameters

After successfully identifying a parameter name, the function registers the parameter and its type in the symbol table. This step is crucial for semantic analysis and code generation phases.

### Error Handling

Throughout the parsing process, the function includes robust error handling to manage unexpected tokens. For example, if the function encounters a closing parenthesis `')'` before reaching the end of the parameter list, it will throw an error indicating a mismatched delimiter.

```cpp
if (check(TokenType::RPAREN))
    error("Unexpected ')' encountered");
```

### Conclusion

The `parseParamList` function plays a vital role in ensuring that parameter lists are correctly parsed during quantum language compilation. By handling various syntax constructs, including type modifiers, C-type keywords, and template arguments, the function provides a comprehensive solution for parameter identification. Its interaction with other components, such as the tokenizer and symbol table, further enhances its utility in the overall compilation process.