# `parseAnd` Function

## Purpose
The `parseAnd` function is crucial in the Quantum Language compiler's parser for handling expressions involving both the logical AND (`&`) and bitwise AND (`&&`) operators. It constructs an Abstract Syntax Tree (AST) node to represent these operations.

## Parameters
- None directly specified in the function signature, but it implicitly relies on global variables such as `pos`, which represents the current position in the token stream.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed expression.

## Detailed Explanation
### Workflow
1. **Initialization**: The function starts by calling `parseBitwise()` to parse the left-hand side of the AND expression. This initial call returns a unique pointer to an `ASTNode`.

2. **Loop for Additional AND Expressions**:
   - A `while` loop is used to handle multiple consecutive AND operations.
   - Inside the loop, `savedPos` is set to the current position in the token stream to allow for potential backtracking.
   - `skipNewlines()` is called to ensure that any newlines between tokens are skipped over.

3. **Check for AND Tokens**:
   - The function checks if the next token is either `TokenType::AND` or `TokenType::AND_AND`.
   - If neither token is found, the loop breaks, indicating the end of the AND expression sequence.

4. **Consume AND Token**:
   - If an AND token is found, it consumes the token using `consume()`. The consumed token is stored in `opToken`.
   - Depending on whether the token type is `TokenType::AND_AND` or `TokenType::AND`, the string representation of the operator (`opStr`) is determined.

5. **Skip Newlines Again**:
   - After consuming the AND token, another call to `skipNewlines()` ensures that any newlines after the operator are ignored.

6. **Parse Right-Hand Side**:
   - The function calls `parseBitwise()` again to parse the right-hand side of the current AND expression. This returns another unique pointer to an `ASTNode`.

7. **Construct AST Node**:
   - An `ASTNode` is constructed with a `BinaryExpr` containing the operator string (`opStr`), the left-hand side (`left`), and the right-hand side (`right`). The line number (`ln`) of the current token is also passed to maintain context.
   - The newly constructed `ASTNode` becomes the new `left` for the next iteration of the loop, allowing for chaining of AND operations.

8. **Return Final AST Node**:
   - Once the loop completes, the final `ASTNode` representing the entire AND expression sequence is returned.

### Edge Cases
- **No AND Tokens**: If there are no `TokenType::AND` or `TokenType::AND_AND` tokens following the initial `parseBitwise()` call, the function will simply return the result of the first parsing without constructing any additional nodes.
- **Mixed Types**: While the function primarily handles `TokenType::AND` and `TokenType::AND_AND`, it can be extended to support other types of binary operators by modifying the logic inside the loop.

### Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the correct sequence of tokens. It uses functions like `current()`, `consume()`, and `check()` to interact with the tokenizer.
- **Abstract Syntax Tree (AST)**: The function constructs and manipulates `ASTNode` objects to build the AST. Each `ASTNode` represents a part of the parsed expression, and the `BinaryExpr` structure encapsulates the operator and its operands.
- **Error Handling**: Although not explicitly shown in the provided code snippet, the function likely interacts with error handling mechanisms within the compiler to manage syntax errors related to AND expressions.

This function effectively parses sequences of AND expressions, ensuring that the resulting AST accurately reflects the structure and intent of the source code.