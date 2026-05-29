# `parseCinStmt`

## Purpose
The `parseCinStmt` function is designed to handle input statements in the Quantum Language compiler's parser, specifically targeting the syntax of `cin >> var`, `cin >> arr[i]`, or `cin >> *(ptr+i)`. It ensures that these statements are correctly parsed without misinterpreting the `>>` operator as a bitwise right shift operator.

## Parameters
- None

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed input statement. If the statement involves calling methods like `cin.ignore()` or `cin.get(...)`, it returns a `BlockStmt` node containing no operations.

## How It Works
1. **Initialization**: The function initializes the line number (`ln`) where the current token is located using `current().line`.

2. **Handling Methods**: 
   - If the next token is a dot (`.`), indicating a method call on `cin`, the function consumes the dot and checks if the following token is an identifier (e.g., `ignore`, `get`, `getline`). If so, it consumes the identifier.
   - It then checks if there is an argument list following the method name. If an argument list is found, it consumes all tokens within the parentheses until it reaches the closing parenthesis. This strategy treats methods like `cin.ignore()`, `cin.get(...)`, and `cin.getline(...)` as no-operations since they do not assign values to variables.
   - After handling the method call and its arguments, the function skips any newlines or semicolons that might follow, returning a `BlockStmt` node.

3. **Parsing Input Statements**:
   - If the next token is not a dot, the function enters a loop to parse multiple input statements separated by `>>`.
   - Inside the loop, it first skips any newlines using `skipNewlines()`.
   - It then checks if the next token is the `>>` operator. If not, it breaks out of the loop.
   - Once the `>>` operator is encountered, the function consumes it and strips any optional `&` character that might precede the variable being assigned.
   - Depending on whether the next token is a star (`*`), the function either parses a dereferenced expression or a simple postfix expression.
     - If a star is found, it indicates a dereferenced expression. The function consumes the star, checks for an opening parenthesis, and then calls `parseExpr()` to safely parse the expression inside the parentheses. After parsing the inner expression, it expects a closing parenthesis and wraps the result in a `DerefExpr` node.
     - If no star is found, it simply parses a postfix expression using `parsePostfixExpr()`.

4. **Edge Cases**:
   - The function handles multiple input statements separated by `>>`.
   - It correctly identifies and parses expressions involving pointers and arrays.
   - It treats methods like `cin.ignore()`, `cin.get(...)`, and `cin.getline(...)` as no-operations.

5. **Interactions with Other Components**:
   - The function interacts with the lexer to consume tokens and determine their type.
   - It uses helper functions like `check()`, `consume()`, `expect()`, and `skipNewlines()` to manage token consumption and error checking.
   - The parsed `ASTNode` can be used by other parts of the compiler to generate executable code or perform semantic analysis.

This function is crucial for accurately parsing input statements in the Quantum Language, ensuring that the `>>` operator is interpreted correctly as part of the input syntax rather than a bitwise shift operation.