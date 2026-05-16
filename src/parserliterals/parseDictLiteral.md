# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is designed to parse dictionary literals from the source code. Dictionary literals are represented using curly braces `{}`, containing key-value pairs. This function ensures that the syntax of dictionary literals is correctly interpreted and constructs an abstract syntax tree (AST) node representing the parsed dictionary.

### Why It Works This Way

The function operates by iterating through the list of tokens until it encounters a closing brace `}` or reaches the end of the input. During each iteration, it checks for various token types to determine whether the current token represents a key, a value, or a spread operator (`...`). The function handles different scenarios such as quoted strings, numbers, bare identifiers, and type keywords as valid keys. If a colon `:` follows a key, it treats the key as a standard key-value pair; otherwise, it considers the key as a shorthand property.

### Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided snippet, but based on typical parser functions, it likely takes a reference to the current position in the token stream and possibly additional context information.

- **Return Value**:
  - Returns an `ASTNodePtr` pointing to a `DictLiteral` node, which represents the parsed dictionary literal.

### Edge Cases

- **Empty Dictionary**: If the parser encounters an empty dictionary `{}`, it should handle this gracefully without throwing errors.
- **Spread Operator at End**: If the spread operator `...` is used at the end of a dictionary literal, the parser should ignore it and stop processing.
- **Missing Colon**: If a key is followed by another key or a closing brace without a colon, the parser should throw an error indicating a syntax issue.
- **Invalid Keys**: Any token type not allowed as a key (e.g., operators, punctuation) should result in an error.

### Interactions With Other Components

- **Tokenizer**: The `parseDictLiteral` function relies on the tokenizer to provide a sequence of tokens representing the source code. It consumes tokens from the tokenizer as part of its parsing process.
- **Error Handling**: The function includes mechanisms to report syntax errors. For example, if the parser expects a colon after a key but finds something else, it will call `error()` to generate an appropriate error message.
- **Abstract Syntax Tree (AST)**: The function constructs an AST by creating `ASTNode` objects for keys and values. These nodes are then added to the `dict.pairs`, forming the structure of the parsed dictionary.

### Detailed Explanation

Here's a more detailed breakdown of how the function parses a dictionary literal:

1. **Initialization**:
   ```cpp
   int ln = current().line;
   expect(TokenType::LBRACE, "Expected '{'");
   ```
   - `ln` stores the line number where the dictionary literal begins.
   - `expect(TokenType::LBRACE, "Expected '{'")` checks if the current token is a left brace `{`. If not, it throws an error indicating that a left brace was expected.

2. **Skip Newlines**:
   ```cpp
   skipNewlines();
   ```
   - Skips any newline characters encountered before the start of the dictionary literal.

3. **Parse Dictionary Literal**:
   ```cpp
   DictLiteral dict;
   while (!check(TokenType::RBRACE) && !atEnd())
   {
       // Handle spread operator
       if (check(TokenType::IDENTIFIER) && current().value == "...")
       {
           consume(); // eat "..."
           auto spreadExpr = parseUnary();
           dict.pairs.emplace_back(nullptr, std::move(spreadExpr));
           skipNewlines();
           if (!match(TokenType::COMMA))
               break;
           skipNewlines();
           if (check(TokenType::RBRACE))
               break;
           continue;
       }

       // Parse key
       ASTNodePtr key;
       bool isShorthand = false;
       if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type) || check(TokenType::TYPE_STRING))
       {
           size_t la = pos + 1;
           while (la < tokens.size() && tokens[la].type == TokenType::NEWLINE)
               la++;

           if (la < tokens.size() && tokens[la].type == TokenType::COLON)
           {
               // Standard key-value pair
               auto keyName = consume().value;
               key = std::make_unique<ASTNode>(StringLiteral{keyName}, ln);
           }
           else if (la < tokens.size() && (tokens[la].type == TokenType::COMMA || tokens[la].type == TokenType::RBRACE))
           {
               // Shorthand property
               auto keyName = consume().value;
               key = std::make_unique<ASTNode>(StringLiteral{keyName}, ln);
               isShorthand = true;
           }
           else
               key = parseExp();
       }

       // Parse value
       ASTNodePtr value = parseExp();

       // Add pair to dictionary
       dict.pairs.emplace_back(std::move(key), std::move(value));

       // Skip newlines and comma
       skipNewlines();
       if (!match(TokenType::COMMA))
           break;
       skipNewlines();
   }
   ```
   - The loop continues until a right brace `}` is encountered or the end of the input is reached.
   - If the spread operator `...` is found, it consumes the token and calls `parseUnary()` to parse the expression following the spread operator. The resulting `ASTNodePtr` is stored in the dictionary with a `nullptr` key.
   - If a standard key is found, it consumes the token and creates an `ASTNode` with a `StringLiteral` type. If the key is followed immediately