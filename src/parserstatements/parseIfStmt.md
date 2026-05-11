# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is responsible for parsing an `if` statement from the token stream and constructing the corresponding Abstract Syntax Tree (AST). This function handles both the traditional `if` statement and the modern `if-with-initializer` syntax introduced in C++17.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` representing the parsed `if` statement node.

## How It Works
### Traditional `if` Statement
For a traditional `if` statement of the form:
```cpp
if (condition) { ... }
```
The function consumes the `if` keyword and checks if the next token is an opening parenthesis (`(`). If so, it parses the condition expression using `parseExpr()` and constructs an `IfStmt` node with the condition and the body parsed by `parseBodyOrStatement()`.

### Modern `if-with-initializer` Syntax
For the modern `if-with-initializer` syntax introduced in C++17:
```cpp
if (auto name = expr) { ... }
```
The function first checks if the token stream starts with `if`, followed by an opening parenthesis (`(`), and then the identifier `auto`. If these conditions are met, it proceeds to parse the initializer expression and assigns it to a variable named `name`.

#### Steps for Parsing `if-with-initializer`:
1. **Consume Tokens**: The function consumes the `if` keyword and the opening parenthesis (`(`).
2. **Parse Initializer**: It expects the next token to be `auto`, followed by a variable name and an assignment operator (`=`). The variable name is extracted and the initializer expression is parsed using `parseExpr()`.
3. **Check Parenthesis Closure**: After parsing the initializer expression, the function expects a closing parenthesis (`)`).
4. **Match Colon**: The function matches a colon (`:`) which separates the initializer from the condition.
5. **Skip Newlines**: Any newline characters are skipped to ensure proper parsing of the subsequent code block.
6. **Parse Condition**: The function parses the condition expression following the colon.
7. **Construct Nodes**: The parsed initializer is used to create a `VarDecl` node, and the condition is used to create an `Identifier` node. These nodes are then combined into an `IfStmt` node along with the body parsed by `parseBodyOrStatement()`.

### Handling `else` Branch
If an `else` branch is present, the function checks for the `else` keyword. Depending on whether the `else` is followed by another `if` (forming an `if-else-if` structure) or a simple colon (`:`), it recursively calls itself or parses the `else` body respectively.

#### Steps for Handling `else` Branch:
1. **Check Else Keyword**: The function checks if the next token is `else`.
2. **Consume Else**: If `else` is found, it consumes the `else` keyword and skips any newlines.
3. **Check Nested If**: If the `else` is followed by `if`, indicating an `if-else-if` structure, the function recursively calls `parseIfStmt()` to parse the nested `if` statement.
4. **Simple Else Body**: If the `else` is followed by a colon, the function matches the colon and skips any newlines before parsing the `else` body using `parseBodyOrStatement()`.

## Edge Cases
- **Missing Parentheses**: If the `if` statement lacks parentheses around the condition, the function will throw an error.
- **Incorrect Assignment Operator**: If the `if-with-initializer` syntax uses a double equals sign (`==`) instead of the single equals sign (`=`) for assignment, the function will throw an error.
- **Misplaced `else`**: If the `else` keyword appears without a preceding `if` statement, or if there's an `else` without a matching `if`, the function will throw an error.
- **Empty Condition**: While not strictly an error, if the condition expression is empty, the function may produce unexpected results depending on how the AST is handled later in the compilation process.

## Interactions With Other Components
The `parseIfStmt` function interacts closely with several other components within the Quantum Language compiler:
- **Tokenizer**: Consumes tokens from the input source code to determine the structure of the `if` statement.
- **Error Handler**: Throws errors when encountering invalid syntax or missing elements in the `if` statement.
- **Abstract Syntax Tree (AST)**: Constructs nodes representing the parsed `if` statement, including variable declarations and conditional expressions.
- **Parsing Context**: Manages the state of the parser, such as the current position in the token stream and line numbers, ensuring consistent parsing across different parts of the source code.

By handling both traditional and modern `if` statement syntax, the `parseIfStmt` function provides flexibility in parsing quantum language code while maintaining robust error checking and AST construction.