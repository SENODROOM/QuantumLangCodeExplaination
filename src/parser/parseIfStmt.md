# `parseIfStmt` Function

## Overview

The `parseIfStmt` function is responsible for parsing an `if` statement in the Quantum Language compiler's parser component. It handles both the traditional `if` statement and the more modern `if-with-initializer` syntax introduced in C++17.

## Parameters

- None explicitly listed in the provided code snippet.

## Return Value

- An `ASTNodePtr` representing the parsed `if` statement node.

## Detailed Explanation

### Functionality

The function begins by recording the current line number (`ln`). It then checks if the next token sequence matches the pattern of an `if-with-initializer` statement, which is defined as:

```
if (auto name = expr) { ... }
```

This pattern involves:
1. An opening parenthesis `(`.
2. The keyword `auto`.
3. A variable identifier.
4. An assignment operator `=`.
5. An expression.
6. A closing parenthesis `)`.

If the pattern is matched, the function proceeds to parse the `if` statement as follows:

1. **Consume Tokens**: The function consumes the opening parenthesis `(`, the keyword `auto`, and the variable identifier.
2. **Parse Initialization Expression**: It then parses the initialization expression using the `parseExpr` function.
3. **Expect Closing Parenthesis**: After parsing the initialization expression, it expects a closing parenthesis `)` and skips any newlines that follow.
4. **Match Colon**: The function then matches the colon `:` that separates the condition from the body of the `if` statement.
5. **Skip Newlines**: It skips any newlines before parsing the body or statement associated with the `if` branch.
6. **Parse Body or Statement**: The `then` part of the `if` statement is parsed using the `parseBodyOrStatement` function.
7. **Optional Else Branch**: If the `else` keyword is encountered, the function proceeds to parse the optional `else` branch. This can either be another `if` statement (nested `if`) or a standalone block of statements.
8. **Construct AST Nodes**:
   - A `VarDecl` node is constructed to represent the declaration of the variable initialized within the `if` statement.
   - An `Identifier` node is created to reference the variable name.
   - An `IfStmt` node is built to encapsulate the entire `if` statement, including the condition, `then` block, and optional `else` branch.

### Edge Cases

1. **Incorrect Syntax**: If the token sequence does not match the expected pattern of an `if-with-initializer`, the function will fail to parse correctly and may produce unexpected results.
2. **Nested `if` Statements**: The function supports nested `if` statements within the `else` branch, allowing for complex conditional logic.
3. **Trailing Whitespace**: Any trailing whitespace or newlines after the `if` statement are skipped to ensure proper parsing.

### Interactions with Other Components

- **Lexer**: The `current()` function retrieves the current token from the lexer, which is essential for determining the type and value of each token.
- **Token Stream Consumption**: The function consumes tokens as they are processed, ensuring that the correct sequence of tokens is matched and parsed.
- **Error Handling**: The function uses `expect` and `match` functions to handle errors gracefully, such as missing parentheses or colons.
- **Abstract Syntax Tree (AST)**: The parsed `if` statement is represented as an `ASTNodePtr`, which is a smart pointer to an `ASTNode`. This allows for efficient memory management and easy manipulation of the parsed structure.

### Example Usage

Here is an example of how the `parseIfStmt` function might be used within the context of the Quantum Language compiler:

```cpp
// Main function to parse a program
void Parser::parseProgram()
{
    while (!isAtEnd())
    {
        auto stmt = parseStmt();
        // Process the statement (e.g., add it to the AST)
        processStatement(stmt);
    }
}

// Helper function to parse a single statement
ASTNodePtr Parser::parseStmt()
{
    if (check(TokenType::IF))
    {
        consume();
        return parseIfStmt();
    }
    // Handle other types of statements
    // ...
}
```

In this example, the `parseProgram` function iterates through the tokens of the input program, calling `parseStmt` to parse individual statements. If the current token is `TokenType::IF`, it calls `parseIfStmt` to handle the `if` statement. This demonstrates how `parseIfStmt` integrates into the overall parsing process of the compiler.