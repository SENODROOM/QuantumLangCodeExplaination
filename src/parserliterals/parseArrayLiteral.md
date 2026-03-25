# `parseArrayLiteral` Function

## Overview

The `parseArrayLiteral` function is part of the Quantum Language Compiler's parser module and is responsible for parsing array literals from the source code. It handles both regular array literals and list comprehensions.

## Parameters and Return Value

- **Parameters**: None
- **Return Type**: `std::unique_ptr<ASTNode>`
  - Returns an ASTNode representing the parsed array literal or list comprehension.

## How It Works

1. **Initialization**:
   - The function starts by recording the current line number (`ln`) using `current().line`.
   - It expects a left bracket `[` token to begin the array literal or list comprehension. If not found, it throws a `ParseError`.

2. **Skip Newlines**:
   - After expecting the left bracket, the function skips any newline tokens using `skipNewlines()`.

3. **Empty Array Check**:
   - The function checks if the next token is a right bracket `]`. If so, it consumes the token and returns an empty `ArrayLiteral` node wrapped in a unique pointer.

4. **First Expression Parsing**:
   - If the array is not empty, the function parses the first expression inside the brackets using `parseExpr()`. This expression can be any valid Quantum Language expression.

5. **List Comprehension Detection**:
   - The function then checks if the next token is a `FOR` keyword. If detected, it proceeds to parse a list comprehension.
     - **Loop Variable(s)**: It collects one or more loop variables (supporting tuple unpacking). Each variable is expected to be either an identifier or a CType keyword.
     - **Iterable Expression**: After collecting the loop variables, it expects an `IN` or `OF` keyword followed by another expression that represents the iterable.
     - **Optional Condition**: If the next token is an `IF` keyword, it parses an optional filtering condition expression.
     - **Right Bracket Expectation**: Finally, it expects a closing right bracket `]` token.

6. **Regular Array Literal Construction**:
   - If no `FOR` keyword is detected, the function constructs a regular `ArrayLiteral` node.
   - It adds the first parsed expression to the `elements` vector of the `ArrayLiteral`.
   - The function continues to parse subsequent expressions separated by commas until encountering a right bracket `]`.

7. **Edge Cases**:
   - **Empty Array**: The function correctly handles empty arrays by returning an empty `ArrayLiteral`.
   - **Syntax Errors**: If the syntax deviates from the expected pattern (e.g., missing comma, incorrect token types), the function throws appropriate `ParseError`.

8. **Interactions with Other Components**:
   - **Tokenizer**: The function relies on the tokenizer to provide the next token during parsing.
   - **ASTNode**: The parsed array literal or list comprehension is represented as an `ASTNode`, which is constructed based on the parsed elements and structure.
   - **Error Handling**: The function uses `expect` and `consume` methods to handle errors and advance the token stream.

## Detailed Steps

1. **Record Line Number**:
   ```cpp
   int ln = current().line;
   ```

2. **Expect Left Bracket**:
   ```cpp
   expect(TokenType::LBRACKET, "Expected '['");
   ```

3. **Skip Newlines**:
   ```cpp
   skipNewlines();
   ```

4. **Check for Empty Array**:
   ```cpp
   if (check(TokenType::RBRACKET))
   {
       consume();
       return std::make_unique<ASTNode>(ArrayLiteral{}, ln);
   }
   ```

5. **Parse First Expression**:
   ```cpp
   auto firstExpr = parseExpr();
   skipNewlines();
   ```

6. **Detect List Comprehension**:
   ```cpp
   if (check(TokenType::FOR))
   {
       consume(); // eat 'for'
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

7. **Construct Regular Array Literal**:
   ```cpp
   ArrayLiteral arr;
   arr.elements.push_back(std::move(firstExpr));
   skipNewlines();
   while (match(TokenType::COMMA))
   {
       // Continue parsing additional elements
   }
   ```

## Conclusion

The `parseArrayLiteral` function effectively parses both regular array literals and list comprehensions in the Quantum Language. Its design ensures robust error handling and correct parsing of complex structures, making it a crucial component of the compiler's parsing phase.