# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`). This function ensures that the correct syntax and semantics of comparison operations are parsed accurately, maintaining the integrity of the quantum program being compiled.

## Parameters
- **None**: The function takes no explicit parameters. It operates on the global state of the parser, accessing the current token through the `current()` method and modifying the parser's position using the `consume()` method.

## Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an abstract syntax tree (AST) node representing the parsed comparison expression. This AST node can be used further in the compilation process to generate executable code or perform semantic analysis.

## Edge Cases
1. **Nested Comparisons**: The function correctly handles nested comparisons, such as `a < b <= c`. In this case, it will parse the expression as `(a < b) <= c`.
2. **Negated Membership Tests**: The function supports both `is not` and `not in` operators. For example, `x is not y` and `z not in w` are valid expressions.
3. **Single Token Operators**: The function handles single-token operators like `<`, `>`, `<=`, and `>=`.

## How It Works
1. **Initial Parsing**: The function starts by parsing the left-hand side of the comparison expression using the `parseShift()` function. This function is responsible for parsing shift expressions, which include terms, factors, and unary operators.
   
   ```cpp
   auto left = parseShift();
   ```

2. **Loop Through Comparison Tokens**: The function then enters a loop that continues as long as the next token is one of the comparison operators (`<`, `>`, `<=`, `>=`, `in`, `is`, `not in`, `is not`). Inside the loop:
   - **Line Number Recording**: The line number of the current token is recorded using `current().line`.
   - **Handling 'is' and 'is not' Operators**:
     - If the current token is `is`, the function consumes it and checks if the next token is `not`. If so, it parses the right-hand side using `parseShift()` and constructs an AST node with the operation `"is not"`. Otherwise, it constructs an AST node with the operation `"is"`.
     
     ```cpp
     if (check(TokenType::IS)) {
         consume(); // eat 'is'
         if (match(TokenType::NOT)) {
             auto right = parseShift();
             left = std::make_unique<ASTNode>(BinaryExpr{"is not", std::move(left), std::move(right)}, ln);
         } else {
             auto right = parseShift();
             left = std::make_unique<ASTNode>(BinaryExpr{"is", std::move(left), std::move(right)}, ln);
         }
         continue;
     }
     ```
   - **Handling 'not in' Operator**:
     - If the current token is `not`, the function consumes it and checks if the next token is `in`. If so, it parses the right-hand side using `parseShift()` and constructs an AST node with the operation `"not in"`.
     
     ```cpp
     if (check(TokenType::NOT)) {
         consume(); // eat 'not'
         if (!match(TokenType::IN))
             throw ParseError("Expected 'in' after 'not'", current().line, current().col);
         auto right = parseShift();
         left = std::make_unique<ASTNode>(BinaryExpr{"not in", std::move(left), std::move(right)}, ln);
         continue;
     }
     ```
   - **Handling 'in' Operator**:
     - If the current token is `in`, the function consumes it and parses the right-hand side using `parseShift()`. It then constructs an AST node with the operation `"in"`.
     
     ```cpp
     if (check(TokenType::IN)) {
         consume();
         auto right = parseShift();
         left = std::make_unique<ASTNode>(BinaryExpr{"in", std::move(left), std::move(right)}, ln);
         continue;
     }
     ```
   - **General Comparison Operators**:
     - For any other comparison operator, the function consumes it, parses the right-hand side using `parseShift()`, and constructs an AST node with the operation represented by the consumed token.
     
     ```cpp
     auto op = consume().value;
     auto right = parseShift();
     left = std::make_unique<ASTNode>(BinaryExpr{op, std::move(left), std::move(right)}, ln);
     ```

3. **Return Parsed Expression**: Once the loop exits (i.e., there are no more comparison tokens), the function returns the unique pointer to the AST node representing the parsed comparison expression.

   ```cpp
   return left;
   ```

## Interactions With Other Components
- **Parser Class**: The `parseComparison` function is part of the `Parser` class, which manages the overall parsing process of the quantum language.
- **Token Stream**: The function interacts with the token stream managed by the `Parser` class to consume tokens and determine their type.
- **Abstract Syntax Tree (AST)**: The function constructs AST nodes using the `ASTNode` class, which represents different elements of the quantum program structure. These nodes are built up recursively based on the parsed comparison expressions.

By handling these comparison operators and their negations, the `parseComparison` function plays a crucial role in ensuring that the quantum