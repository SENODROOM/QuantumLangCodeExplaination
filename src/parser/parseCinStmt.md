# `parseCinStmt` Function

## Purpose
The `parseCinStmt` function is designed to handle input statements in the form of `cin >> ...`, specifically targeting variable assignments, array element access, pointer dereferencing, and multiple variable assignments using `cin`.

## Why It Works This Way
This approach is necessary because the `>>` operator in C++ can also be interpreted as a bitwise right shift operator when used outside of input streams. To correctly parse input statements without misinterpreting `>>` as a shift operator, the parser must explicitly look for the `cin` keyword followed by a `>>` token. If the target of the assignment starts with `*` and parentheses `(`, the parser will treat it as a pointer dereference operation, wrapping the parsed expression in a `DerefExpr`. For other cases, it parses the expression postfixed with `>>`.

## Parameters/Return Value
### Parameters
- None

### Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed statement. In case of `cin.ignore()` or similar methods, it returns a `BlockStmt` node.

## Edge Cases
1. **Pointer Dereferencing**: The function handles cases where the target of the assignment is a pointer (`*var`) or an array element accessed through a pointer (`arr[i]`, `*(ptr+i)`), ensuring that these expressions are correctly wrapped in a `DerefExpr`.
2. **Multiple Variable Assignments**: The function supports parsing multiple variables separated by spaces (`cin >> var1 >> var2;`).
3. **No-Operation Statements**: The function treats calls to `cin.ignore()`, `cin.get(...)`, `cin.getline(...)`, and similar methods as no-operations, returning a `BlockStmt` node.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens such as `TokenType::DOT`, `TokenType::IDENTIFIER`, `TokenType::LPAREN`, `TokenType::RPAREN`, `TokenType::RSHIFT`, and `TokenType::BIT_AND`.
- **Expression Parser**: When encountering a dereferenced expression (`*(expr)`), the function uses `parseExpr()` to safely parse the expression within the parentheses.
- **Error Handling**: The function includes error handling to ensure that the expected closing parenthesis `)` is found after a dereferenced expression.

## Detailed Implementation
Here's a more detailed breakdown of how the function operates:

1. **Line Number Tracking**:
   ```cpp
   int ln = current().line;
   ```
   - Tracks the line number of the current token to maintain context for any errors or nodes created during parsing.

2. **Handling No-Operation Statements**:
   ```cpp
   if (check(TokenType::DOT)) {
       consume(); // eat '.'
       if (check(TokenType::IDENTIFIER))
           consume(); // eat method name (ignore, get, getline, peek, ...)
       // eat argument list if present
       if (check(TokenType::LPAREN)) {
           consume(); // eat '('
           int depth = 1;
           while (!atEnd() && depth > 0) {
               if (check(TokenType::LPAREN))
                   depth++;
               else if (check(TokenType::RPAREN))
                   depth--;
               consume();
           }
       }
       while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
           consume();
       return std::make_unique<ASTNode>(BlockStmt{}, ln);
   }
   ```
   - Checks if the current token is a dot (`.`), which indicates a call to a method like `cin.ignore()`.
   - Consumes the dot and the method name.
   - Optionally consumes arguments enclosed in parentheses.
   - Skips any newlines or semicolons that follow.
   - Returns a `BlockStmt` node indicating a no-operation.

3. **Parsing Input Statements**:
   ```cpp
   BlockStmt block;
   while (true) {
       skipNewlines();
       if (!check(TokenType::RSHIFT))
           break;
       consume(); // eat >>
       if (check(TokenType::BIT_AND))
           consume(); // strip optional &
   ```
   - Initializes a `BlockStmt` to hold multiple parsed input statements.
   - Continuously skips newlines until a non-newline token is encountered.
   - Breaks out of the loop if the next token is not `TokenType::RSHIFT`, indicating the end of input statements.

4. **Handling Pointer Dereferencing**:
   ```cpp
   ASTNodePtr lval;
   if (check(TokenType::STAR)) {
       consume(); // eat *
       if (check(TokenType::LPAREN)) {
           consume();                // eat (
           auto inner = parseExpr(); // safe: balanced parens, >> inside parens is shift
           expect(TokenType::RPAREN, "Expected ')' after dereferenced expression");
           DerefExpr de;
           de.operand = std::move(inner);
           lval = std::make_unique<DerefExpr>(std::move(de));
       } else {
           lval = parseLVal(); // parse lvalue normally
       }
   } else {
       lval = parseLVal(); // parse lvalue normally
   }
   ```
   - Checks if the next token is `TokenType::STAR`, indicating a pointer dereference.
   - Consumes the star (`*`) and optionally the ampersand (`&`).
   - Parses the left-hand side (l-value) of the assignment.
   - If the l-value starts with `*` and parentheses, wraps the parsed expression in a `DerefExpr`.

5. **Assigning Values**:
   ```cpp
   consume(); // eat <<
   ASTNodePtr expr = parseExpr();
   block.add(std::make_unique<InputStmt>(std::