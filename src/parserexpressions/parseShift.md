# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters
- None explicitly stated in the provided code snippet. However, based on typical parser implementations, the function likely takes no parameters directly. Instead, it interacts with global state such as `tokens`, `current()`, and `consume()`.

## Return Value
- The function returns a unique pointer to an `ASTNode` representing the parsed shift expression. If no shift operation is found, it returns the result of `parseAddSub()`.

## How It Works
The `parseShift` function begins by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:
- It checks if the next token after the current operator is an assignment (`TokenType::ASSIGN`). If so, it breaks out of the loop because these are treated as compound assignment operators (e.g., `<<=`).
- It records the line number of the current token using `current().line`.
- It consumes the current token to get the operator value (`op`).
- It calls `parseAddSub()` again to parse the right-hand side of the shift expression.
- It constructs a new `ASTNode` containing a `BinaryExpr` with the operator, the left-hand side, and the right-hand side. This node represents the parsed shift expression.
- It updates the left-hand side (`left`) with the newly constructed node.

This process repeats until there are no more shift operators, ensuring that all shift operations are correctly parsed and ordered within the expression.

## Edge Cases
- **No Shift Operators**: If there are no shift operators in the input sequence, the function simply returns the result of `parseAddSub()`. This allows for fall-through into subsequent parsing stages.
- **Compound Assignment Operators**: The function breaks out of the loop when encountering a compound assignment operator immediately following a shift operator. This prevents incorrect parsing of expressions like `a <<= b`.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens (`tokens`). It uses `current()` and `consume()` methods to access and manipulate these tokens.
- **Parse Tree Construction**: The function constructs an abstract syntax tree (AST) using `ASTNode` and `BinaryExpr`. These structures represent the parsed expression and facilitate further semantic analysis and code generation.
- **Error Handling**: While not shown in the snippet, the function likely includes error handling mechanisms to manage unexpected tokens or malformed expressions. This could involve reporting errors and possibly recovering from them.

Overall, the `parseShift` function plays a crucial role in accurately parsing shift expressions within the Quantum Language compiler, ensuring that they are correctly represented in the AST and maintaining the proper order of operations.