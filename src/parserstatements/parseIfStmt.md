# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is designed to parse an `if` statement from the token stream and construct the corresponding Abstract Syntax Tree (AST). It supports both traditional `if` statements and the more modern `if-with-initializer` syntax introduced in C++17. This function ensures that the parsed `if` statement adheres to the language's grammar rules and constructs a valid AST node representing the `if` statement.

## Parameters
- **None**: The function does not take any explicit parameters. It operates directly on the global state of the parser, which includes the current position in the token stream (`pos`) and the list of tokens (`tokens`).

## Return Value
- **ASTNodePtr**: The function returns a pointer to an `ASTNode` object representing the parsed `if` statement. This node can be either a simple `IfStmt` or an `IfStmt` wrapped inside a `VarDecl` if the `if-with-initializer` syntax is used.

## Edge Cases
- **Empty Else Clause**: If the `else` clause is empty, the function correctly handles it without throwing errors.
- **Nested If Statements**: The function can handle nested `if` statements within the `else` block, recursively calling itself to parse each nested `if`.
- **Misplaced Tokens**: If the tokens do not conform to the expected `if` statement structure, the function will throw appropriate exceptions indicating the error.

## Interactions with Other Components
- **Token Stream Consumption**: The function consumes tokens from the global token stream as it progresses through the parsing process. This interaction is crucial for advancing the parser's state and ensuring that subsequent functions receive the correct sequence of tokens.
- **Error Handling**: The function uses various helper methods like `expect`, `match`, and `consume` to check and enforce the grammar rules of the `if` statement. These methods interact with the token stream and provide feedback if the syntax is incorrect.
- **Abstract Syntax Tree Construction**: The function constructs an `ASTNode` based on the parsed `if` statement. This involves creating nodes for variable declarations, condition expressions, and body statements. The constructed AST nodes are then returned to the caller, allowing further processing or code generation.

## Implementation Details
Here’s a breakdown of how the `parseIfStmt` function works:

1. **Initialization**:
   - The function starts by recording the current line number (`ln`) using `current().line`.

2. **Check for `if-with-initializer` Syntax**:
   - It checks if the next few tokens indicate the presence of the `if-with-initializer` syntax (`if (auto name = expr)`).
   - If found, it proceeds to parse the initializer expression and the condition expression.

3. **Parse Initializer Expression**:
   - Consumes the opening parenthesis `(`.
   - Consumes the keyword `auto`.
   - Parses the variable name identifier.
   - Consumes the assignment operator `=`.
   - Parses the initialization expression (`expr`).
   - Consumes the closing parenthesis `)`.

4. **Parse Condition and Body**:
   - Expects a colon `:` after the closing parenthesis.
   - Skips any newlines to ensure proper alignment.
   - Parses the body of the `if` statement or another `if` statement if the `else` part contains a nested `if`.

5. **Handle Optional `else` Clause**:
   - Checks if the next token is `else`. If so, it parses the optional `else` clause.
   - If the `else` clause contains another `if`, it recursively calls `parseIfStmt()` to parse the nested `if`.
   - Otherwise, it expects a colon `:` followed by the body of the `else` clause.

6. **Construct AST Node**:
   - If the `if-with-initializer` syntax was used, it constructs a `VarDecl` node containing the variable declaration and the initialization expression.
   - Constructs an `IfStmt` node with the condition expression and the parsed bodies (then and else).
   - Returns the constructed AST node.

By following these steps, the `parseIfStmt` function effectively parses `if` statements in the Quantum Language compiler, supporting both traditional and modern syntax, and constructs a valid AST representation of the parsed statements.