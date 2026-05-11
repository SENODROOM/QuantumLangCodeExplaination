# `parseOr` Function

## Purpose
The `parseOr` function is part of the parser in the Quantum Language compiler and is responsible for parsing expressions involving logical OR (`or`), logical AND (`&&`), and null-coalescing (`??`) operators. It constructs an Abstract Syntax Tree (AST) node representing these operations.

## Parameters
- None explicitly defined as parameters. The function uses global variables such as `pos`, which tracks the current position in the token stream, and `current()`, which returns the current token.

## Return Value
- Returns a `std::unique_ptr<ASTNode>` representing the parsed expression tree.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseAnd()` to parse the left-hand side of the OR expression. This ensures that any AND expressions are parsed first before moving on to OR expressions.

2. **Loop for Subsequent OR Expressions**:
   - Inside a loop, the function checks if the next token is one of the OR operators (`or`, `||`, `??`). If not, it breaks out of the loop.
   - It saves the current position (`savedPos`) and skips any newlines using `skipNewlines()`.

3. **Consuming Operator Token**:
   - When an OR operator is found, the function consumes the token using `consume()`. Depending on the type of the consumed token (`TokenType::OR_OR`), it sets the operator string (`opStr`) to either `"or"` or the actual value of the token.

4. **Recursive Parsing**:
   - After consuming the OR operator, the function again calls `parseAnd()` to parse the right-hand side of the OR expression.
   - It then creates a new `ASTNode` with a `BinaryExpr` containing the operator string, the left-hand side (`left`), and the right-hand side (`right`). This node represents the OR operation in the AST.

5. **Updating Left Node**:
   - The newly created OR node becomes the new left-hand side for the next iteration of the loop, allowing for multiple consecutive OR operations to be parsed correctly.

6. **Edge Cases Handling**:
   - The function handles cases where there are no more OR operators by breaking out of the loop and returning the final parsed expression.
   - It also handles potential errors gracefully by checking for valid tokens and resetting the position if necessary.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing. Tokens are used to identify the type of operation being performed.
- **AST Construction**: The function constructs nodes in the AST based on the parsed expressions. These nodes are then used by higher-level functions to build the complete abstract syntax tree.
- **Error Handling**: The function includes error handling mechanisms to manage unexpected token types or positions, ensuring robust parsing even when input is malformed.

Overall, the `parseOr` function plays a crucial role in the parser's ability to handle complex logical expressions within the Quantum Language. By recursively parsing sub-expressions and combining them with OR operations, it builds a comprehensive AST that accurately reflects the structure and intent of the source code.