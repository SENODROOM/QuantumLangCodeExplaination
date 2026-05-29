# `parseArrayLiteral` Function

## Overview

The `parseArrayLiteral` function is a crucial part of the Quantum Language Compiler's parser module. It processes array literals from the source code and converts them into Abstract Syntax Tree (AST) nodes. This function supports both regular array literals and list comprehensions.

### Parameters
- **None**: The function operates directly on the global state maintained by the parser, such as the current token and the input stream.

### Return Value
- **`std::unique_ptr<ASTNode>`**: Returns a unique pointer to an `ASTNode` representing the parsed array literal or list comprehension. If the array is empty, it returns a node for an empty array literal.

### Edge Cases
1. **Empty Array**: If the array literal is empty (`[]`), the function correctly identifies it and returns an AST node for an empty array literal.
2. **Single Element**: If the array contains only one element, the function parses that element and constructs a regular array literal.
3. **List Comprehension**: The function can handle list comprehensions of the form `[expr for var in iterable (if cond)?]`. It correctly identifies the syntax and parses the expression, loop variables, iterable, and optional condition.

### Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing.
- **Error Handling**: The function uses error handling mechanisms provided by the parser to report syntax errors, such as missing brackets or incorrect variable types in list comprehensions.
- **Expression Parsing**: The function calls `parseExpr()` to parse individual expressions within the array or list comprehension.

## Detailed Explanation

### Step-by-Step Parsing Process

1. **Determine Line Number**:
   ```cpp
   int ln = current().line;
   ```
   - Retrieves the line number where the array literal starts.

2. **Expect Left Bracket**:
   ```cpp
   expect(TokenType::LBRACKET, "Expected '['");
   skipNewlines();
   ```
   - Ensures that the next token is a left bracket (`[`).
   - Skips any newlines after the opening bracket.

3. **Check for Empty Array**:
   ```cpp
   if (check(TokenType::RBRACKET))
   {
       consume();
       return std::make_unique<ASTNode>(ArrayLiteral{}, ln);
   }
   ```
   - If the next token is a right bracket (`]`), indicating an empty array, the function consumes the token and returns an AST node for an empty array literal.

4. **Parse First Expression**:
   ```cpp
   auto firstExpr = parseExpr();
   skipNewlines();
   ```
   - Parses the first expression inside the array using `parseExpr()`.
   - Skips any newlines after the first expression.

5. **Check for List Comprehension**:
   ```cpp
   if (check(TokenType::FOR))
   {
       consume(); // eat 'for'
       // Collect loop variable(s) — support tuple unpacking: for k, v in ...
       std::vector<std::string> vars;
       auto readVar = [&]()
       {
           if (check(TokenType::IDENTIFIER))
               vars.push_back(consume().value);
           else if (isCTypeKeyword(current().type))
               vars.push_back(consume().value);
           else
               vars.push_back(expect(TokenType::IDENTIFIER, "Expected variable in comprehension").value);
       };
       readVar();
       while (match(TokenType::COMMA))
           readVar();

       if (!match(TokenType::IN) && !match(TokenType::OF))
           throw ParseError("Expected 'in' in list comprehension", current().line, current().col);

       auto iterable = parseExpr();
       skipNewlines();

       // Optional filter: if condition
       ASTNodePtr condition;
       if (check(TokenType::IF))
       {
           consume();
           condition = parseExpr();
           skipNewlines();
       }

       expect(TokenType::RBRACKET, "Expected ']'");
       ListComp lc;
       lc.expr = std::move(firstExpr);
       lc.vars = std::move(vars);
       lc.iterable = std::move(iterable);
       lc.condition = std::move(condition);
       return std::make_unique<ASTNode>(std::move(lc), ln);
   }
   ```
   - If the next token is `FOR`, the function assumes it's a list comprehension.
   - Consumes the `FOR` token.
   - Reads loop variables, supporting tuple unpacking.
   - Ensures the presence of `IN` or `OF` followed by the iterable expression.
   - Optionally parses a condition using `if`.
   - Consumes the closing right bracket (`]`) and constructs a `ListComp` object containing the parsed elements, loop variables, iterable, and condition.
   - Returns an AST node for the list comprehension.

6. **Regular Array Literal**:
   ```cpp
   ArrayLiteral arr;
   arr.elements.push_back(std::move(firstExpr));
   skipNewlines();
   while (match(TokenType::COMMA))
   {
       ski
   ```
   - Initializes an `ArrayLiteral` object.
   - Adds the first expression to the array elements.
   - Continuously skips newlines and matches commas to parse additional expressions until a closing right bracket (`]`) is encountered.
   - Constructs an AST node for the regular array literal and returns it.

This function ensures robust parsing of array literals and list comprehensions, providing a solid foundation for further processing in the compiler.