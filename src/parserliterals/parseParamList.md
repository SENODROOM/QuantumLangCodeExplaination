# `parseParamList` Function

## Overview

The `parseParamList` function is crucial for parsing parameter lists within the quantum language compiler. It ensures that the syntax of function declarations and definitions is accurately interpreted, correctly identifying and processing each parameter. This function plays a vital role in maintaining the integrity and correctness of the compiled code.

## Parameters

- **None**

## Return Value

- **std::vector<std::string>**: A vector containing the names of the parsed parameters.

## Detailed Explanation

### Syntax Parsing

The function begins by expecting an opening parenthesis (`(`) using the `expect` method. If the expected token is not found, it throws an error indicating that a left parenthesis was expected.

```cpp
expect(TokenType::LPAREN, "Expected '('");
```

### Parameter Loop

It then enters a loop that continues until either a closing parenthesis (`)` is encountered or the end of the input stream is reached. Within this loop, it processes each parameter:

```cpp
while (!check(TokenType::RPAREN) && !atEnd()) {
```

#### Handling `const` Modifier

If the current token is a `const` modifier, it consumes the token to skip it:

```cpp
if (check(TokenType::CONST))
  consume();
```

#### Identifying C-Type Keywords

The function checks if the current token or the next token(s) are C-type keywords (like `int`, `char`, etc.). If so, it consumes these tokens to identify the parameter type:

```cpp
bool hasCType = false;
while ((isCTypeKeyword(current().type) || check(TokenType::CONST)) &&
       pos + 1 < tokens.size() &&
       (isCTypeKeyword(tokens[pos + 1].type) || tokens[pos + 1].type == TokenType::IDENTIFIER ||
        tokens[pos + 1].type == TokenType::STAR || tokens[pos + 1].type == TokenType::BIT_AND)) {
  consume(); // eat return/param type keyword or const
  hasCType = true;
}
```

#### Pointer and Reference Qualifiers

If the parameter type is identified as having pointer (`*`) or reference (`&`) qualifiers, the function consumes these tokens as well:

```cpp
if (hasCType) {
  while (check(TokenType::STAR) || check(TokenType::BIT_AND)) {
    consume(); // eat pointer/ref qualifier on type
  }
}
```

#### Identifier Type Before Name

To detect the actual parameter name, the function looks ahead for an identifier following any pointer or reference qualifiers. This is done using a lookahead mechanism:

```cpp
if (check(TokenType::IDENTIFIER)) {
  size_t la = pos + 1;
  // Skip template arguments <...> in lookahead
  if (la < tokens.size() && tokens[la].type == TokenType::LT) {
    int tdepth = 0;
    while (la < tokens.size()) {
      if (tokens[la].type == TokenType::LT)
        tdepth++;
      else if (tokens[la].type == TokenType::GT) {
        tdepth--;
        la++;
        if (tdepth <= 0)
          break;
        continue;
      } else if (tokens[la].type == TokenType::RSHIFT) {
        tdepth -= 2;
        la++;
        if (tdepth <= 0)
          break;
        continue;
      }
      la++;
    }
  }
  while (la < tokens.size() && (tokens[la].type == TokenType::BIT_AND ||
                                tokens[la].type == TokenType::STAR ||
                                tokens[la].type == TokenType::IDENTIFIER)) {
    consume(); // eat pointer/ref qualifier on type
    la++;
  }
  params.push_back(consume().lexeme); // Add the parameter name to the list
}
```

### Edge Cases

- **Empty Parameter List**: If the function encounters a closing parenthesis immediately after the opening one without any parameters, it will simply return an empty vector.
- **Template Arguments**: The function correctly handles template arguments within parameter types, such as `unique_ptr<int[]>`, `shared_ptr<Foo>`, etc., by skipping over them during the lookahead.

### Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide a sequence of tokens representing the source code. It uses methods like `current()` and `consume()` to interact with the tokenizer.
- **Error Handling**: The function includes error handling mechanisms to ensure that the correct syntax is maintained throughout the parsing process. If an unexpected token is encountered, it throws an appropriate error message.

Overall, the `parseParamList` function is designed to handle complex parameter types and names, ensuring that the quantum language compiler can accurately interpret and process function declarations and definitions. Its robustness and flexibility make it an integral part of the compiler's functionality.