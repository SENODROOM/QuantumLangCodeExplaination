# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters
- None explicitly listed in the provided code snippet. However, based on typical parser implementations, it likely takes a reference to the token stream and possibly additional context information.

## Return Value
- The function returns a unique pointer to an `ASTNode` representing the parsed shift expression. If no shift operation is found, it returns the result of the initial call to `parseAddSub()`.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. This function is responsible for parsing addition and subtraction expressions, which are typically evaluated before shifts according to the language's operator precedence rules.

2. **Loop for Shift Operations**: The function then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:
   - It checks if the next token is followed by an assignment token (`TokenType::ASSIGN`). If so, it breaks out of the loop because these are compound assignment operators (`<<=` and `>>=`), and they should be handled separately by the `parseAssignment()` function.
   - It consumes the current token (which is either `<<` or `>>`) and stores its value.
   - It recursively calls `parseAddSub()` again to parse the right-hand side of the shift expression.
   - It creates a new `ASTNode` using the `std::make_unique` function, wrapping the binary expression (`BinaryExpr`) that includes the shift operator, the left-hand side, and the right-hand side. The line number (`ln`) is also stored in the `ASTNode`.

3. **Return Statement**: After exiting the loop, the function returns the unique pointer to the `ASTNode` that represents the entire shift expression.

## Edge Cases
- **No Shift Operators**: If there are no shift operators in the input, the function simply returns the result of the initial call to `parseAddSub()`, effectively ignoring the shift parsing logic.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators (`<<=` and `>>=`) by breaking out of the loop when such operators are encountered.

## Interactions with Other Components
- **Operator Precedence**: The function relies on the `parseAddSub()` function to handle addition and subtraction expressions first, ensuring that the correct order of operations is maintained.
- **Token Consumption**: The function consumes tokens from the token stream using the `consume()` method, which is presumably defined elsewhere in the parser implementation.
- **Error Handling**: While not shown in the provided code snippet, typical parsers would have error handling mechanisms in place to manage unexpected tokens or syntax errors within the shift expression parsing process.

This function is crucial for accurately parsing shift expressions in the Quantum Language compiler, ensuring that the resulting Abstract Syntax Tree (AST) reflects the intended order of operations and correctly handles both simple and compound shift expressions.