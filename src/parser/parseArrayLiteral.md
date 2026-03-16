# parseArrayLiteral

The `parseArrayLiteral` function is an essential part of the Quantum Language compiler, tasked with parsing array literals and list comprehensions from the source code into Abstract Syntax Tree (AST) nodes. This function ensures that the syntax rules for arrays and comprehensions are accurately interpreted and represented within the AST, facilitating subsequent compilation steps.

## Functionality

The primary responsibility of `parseArrayLiteral` is to parse expressions enclosed within square brackets (`[]`). It distinguishes between regular array literals and list comprehensions based on the presence of keywords like `for`, `in`, or `of`. If a list comprehension is detected, it further parses the loop variables, iterable, and optional condition before constructing an appropriate AST node.

### Parameters/Return Value

- **Parameters**:
  - None explicitly defined; operates directly on global state managed by the parser.

- **Return Value**:
  - An `std::unique_ptr<ASTNode>` representing either an `ArrayLiteral` or a `ListComp` depending on the parsed input.

## Edge Cases

1. **Empty Array**: The function correctly handles empty array literals (`[]`) by returning an `ArrayLiteral` node without any elements.
2. **Single Element**: For arrays containing a single element, the function constructs an `ArrayLiteral` with one element.
3. **Tuple Unpacking**: List comprehensions support tuple unpacking, allowing multiple variables to be declared as loop variables (`for k, v in ...`).
4. **Optional Condition**: The function allows for an optional condition within list comprehensions (`[expr for var in iterable if cond]`).

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide tokens such as `LBRACKET`, `RBRACKET`, `IDENTIFIER`, `FOR`, `IN`, etc., which are used to determine the structure of the array or comprehension.
- **parseExpr()**: This helper function is invoked to parse individual expressions within the array or comprehension. The result is stored in `firstExpr` and subsequently used to build the AST node.
- **Error Handling**: The function throws `ParseError` exceptions when encountering unexpected tokens or structures, ensuring robust error reporting during the parsing phase.

## Detailed Implementation

Here’s a breakdown of how the function processes different scenarios:

1. **Empty Array**:
   ```cpp
   if (check(TokenType::RBRACKET))
   {
       consume();
       return std::make_unique<ASTNode>(ArrayLiteral{}, ln);
   }
   ```
   If the next token is `RBRACKET`, indicating an empty array, the function consumes it and returns an `ArrayLiteral` node.

2. **Regular Array Literal**:
   ```cpp
   ArrayLiteral arr;
   arr.elements.push_back(std::move(firstExpr));
   skipNewlines();
   while (match(TokenType::COMMA))
   {
       arr.elements.push_back(parseExpr());
       skipNewlines();
   }
   expect(TokenType::RBRACKET, "Expected ']'");
   return std::make_unique<ASTNode>(arr, ln);
   ```
   For non-comprehension arrays, the function continues to parse comma-separated expressions until `RBRACKET` is encountered. Each parsed expression is added to the `elements` vector of the `ArrayLiteral`.

3. **List Comprehension**:
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
   When a `FOR` keyword is found, the function enters list comprehension mode. It collects loop variables using `readVar()`, which supports both identifiers and CType keywords. After collecting variables, it expects either `IN` or `OF` followed by another expression representing the iterable. Optionally, it checks for a `condition` after `IF`. Finally, it constructs a `ListComp` node with all parsed components and returns it.

This implementation ensures that the Quantum Language compiler can handle both simple array literals and complex list comprehensions, providing a flexible and powerful way to represent data structures in the source code.