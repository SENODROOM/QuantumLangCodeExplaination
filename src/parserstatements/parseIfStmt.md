# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is designed to parse an `if` statement from the token stream and construct the corresponding Abstract Syntax Tree (AST). It supports both traditional `if` statements and the more modern `if-with-initializer` syntax introduced in C++17.

## Parameters
- **None**: The function operates directly on the global token stream and modifies its position as it parses.

## Return Value
- **ASTNodePtr**: A pointer to the root of the AST representing the parsed `if` statement.

## How It Works
The function begins by identifying the starting line number of the `if` statement using `current().line`. It then checks if the next tokens in the stream indicate the presence of an `if-with-initializer` syntax (`if (auto varName = expr)`).

### Step-by-Step Parsing:
1. **Check for Initializer**:
   - The function first verifies if the current token is an opening parenthesis (`(`).
   - It then checks if the next three tokens form the pattern `auto identifier = expression`.

2. **Parse Initializer Expression**:
   - If the pattern matches, the function consumes the tokens up to the assignment operator (`=`).
   - It extracts the variable name (`varName`) and parses the initialization expression (`initExpr`).

3. **Construct Variable Declaration Node**:
   - After parsing the initializer, the function creates an AST node for the variable declaration using `VarDecl`.
   - This node includes the variable name, the initialization expression, and the type specifier `"auto"`.

4. **Parse Condition Expression**:
   - The condition expression is constructed using the variable name extracted earlier, creating an `Identifier` node.

5. **Parse Body or Statement**:
   - The function then expects a colon (`:`) indicating the start of the body of the `if` statement.
   - It skips any new lines and calls `parseBodyOrStatement()` to parse the body of the `if` block.

6. **Handle Optional Else Clause**:
   - If the next token is `else`, the function processes the optional `else` clause.
   - It checks if the following token is `if`, which indicates a nested `if` statement within the `else` block.
   - Depending on whether there's a nested `if` or not, it either recursively calls `parseIfStmt()` or parses another body or statement for the `else` block.

7. **Construct Full If Statement Node**:
   - Finally, the function constructs the full `if` statement node using `IfStmt`.
   - This node contains the condition expression, the body of the `if` block, and the optional `else` branch.

8. **Emit Code**:
   - The constructed AST nodes represent the code structure of the `if` statement, including variable declarations and conditional logic.

## Edge Cases
- **Missing Parentheses**: If the parentheses around the condition are missing, the function will throw an error indicating the expected closing parenthesis.
- **Misplaced Else**: If the `else` keyword is used without a preceding `if` statement, the function will raise an error.
- **Nested Ifs**: The function correctly handles nested `if` statements within the `else` block, ensuring proper parsing and AST construction.

## Interactions with Other Components
- **Token Stream Consumption**: The function consumes tokens from the global token stream as it progresses through the `if` statement.
- **Error Handling**: Utilizes functions like `expect()` and `match()` to ensure correct syntax and handle errors gracefully.
- **Body Parsing**: Relies on `parseBodyOrStatement()` to parse the body of the `if` block, which could be either a single statement or a compound statement.
- **Abstract Syntax Tree Construction**: Constructs AST nodes using classes like `VarDecl` and `IfStmt`, which represent different parts of the `if` statement's structure.

This comprehensive approach ensures that the `if` statement is accurately parsed and represented in the AST, facilitating further compilation steps.