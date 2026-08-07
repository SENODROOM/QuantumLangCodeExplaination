# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function extends the parsing capabilities of the existing equality expression parser, allowing it to handle more complex expressions involving these bitwise operators.

## Parameters/Return Value
- **Parameters**: None explicitly stated in the provided code snippet, but it implicitly uses global state variables like `current()` and `consume()`.
- **Return Value**: The function returns a unique pointer to an `ASTNode` representing the parsed bitwise expression. If there are no bitwise operations present, it simply returns the result of `parseEquality()`.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseEquality()` to get the left-hand side of the bitwise expression. This initial call sets up the base expression to which subsequent bitwise operations will be applied.

2. **Loop for Bitwise Operations**: 
   - The function enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`).
   - Inside the loop:
     - It records the current line number using `current().line`.
     - It consumes the current token using `consume()`, which means it moves to the next token in the input stream and retrieves its value.
     - It recursively calls `parseEquality()` again to parse the right-hand side of the bitwise operation.
     - It creates a new `ASTNode` with a `BinaryExpr` containing the operator value, the previously parsed left-hand side, and the newly parsed right-hand side.
     - It updates the left-hand side to this new node, preparing for potential further bitwise operations.

3. **Edge Cases**:
   - If there are no bitwise operators following the initial equality expression, the function simply returns the result of `parseEquality()`.
   - The function assumes that the input tokens are correctly formatted and that `parseEquality()` can handle any valid equality expression.

4. **Interactions with Other Components**:
   - The function relies on the global state variables `current()` and `consume()` to manage the token stream.
   - It interacts with the `ASTNode` class to construct abstract syntax trees (ASTs) representing the parsed expressions.
   - The function also utilizes helper functions like `check()` to determine if the next token matches a specific type, ensuring that only valid bitwise operations are processed.

This implementation ensures that the `parseBitwise` function can correctly parse and construct ASTs for expressions involving bitwise operations, leveraging the existing parser infrastructure for equality expressions.