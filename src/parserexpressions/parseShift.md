# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters
- **None**: The function does not take any explicit parameters. It operates on global state managed by the parser, specifically the current token position and the list of tokens being parsed.

## Return Value
- **Returns an `std::unique_ptr<ASTNode>`**: The function returns a unique pointer to an abstract syntax tree (AST) node representing the parsed shift expression. If no shift operation is found, it returns the result of parsing an addition or subtraction expression (`parseAddSub()`).

## How It Works
The `parseShift` function starts by calling `parseAddSub()` to parse the initial expression, which could be either an addition or subtraction expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:
- It checks if the next token is followed by an assignment token (`TokenType::ASSIGN`). If so, it breaks out of the loop because these are considered compound assignment operators (`<<=` and `>>=`).
- It consumes the current token, which is either `<<` or `>>`, and stores its value.
- It recursively calls `parseAddSub()` again to parse the right-hand side of the shift expression.
- It creates a new `ASTNode` containing a `BinaryExpr` object, which represents the binary shift operation. The `BinaryExpr` object includes the operator, the left-hand side expression, and the right-hand side expression.
- It updates the line number (`ln`) to reflect the current line where the shift operation occurs.
- It assigns the new AST node back to `left`, effectively chaining the shift operations together.

## Edge Cases
- **No Shift Operations**: If there are no shift operations following the initial expression, the function simply returns the result of `parseAddSub()`.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators like `<<=` and `>>=`, breaking out of the loop when such operators are encountered.

## Interactions With Other Components
- **Parsing Context**: This function relies on the global parsing context managed by the parser, including the current token position (`pos`) and the list of tokens (`tokens`).
- **Error Handling**: While not shown in the provided code snippet, the function likely interacts with error handling mechanisms within the parser to manage unexpected tokens or malformed expressions.
- **Abstract Syntax Tree Construction**: The function constructs an AST by creating `ASTNode` objects, which represent different parts of the parsed expression. These nodes are then used by higher-level functions to build more complex expressions or statements.

This implementation ensures that shift expressions are correctly parsed and represented in the AST, maintaining the integrity and correctness of the compiled quantum program.