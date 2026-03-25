# `parseAnd` Function

## Purpose
The `parseAnd` function is part of the Quantum Language compiler's parser and is responsible for parsing expressions involving the logical AND operator (`&`) and the bitwise AND operator (`&&`). It constructs an abstract syntax tree (AST) node representing these operations.

## Parameters/Return Value
- **Parameters**: None explicitly defined in the function signature.
- **Return Value**: A unique pointer to an ASTNode object representing the parsed expression.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseBitwise()`, which parses the left-hand side of the AND operation. This result is stored in the variable `left`.

2. **Loop for Multiple ANDs**: The function enters a loop that continues as long as it encounters either the `TokenType::AND` or `TokenType::AND_AND` token. These tokens represent the logical AND (`&`) and bitwise AND (`&&`) operators respectively.

3. **Saving Position**: Inside the loop, the current position in the input stream is saved using `size_t savedPos = pos;`. This allows the parser to backtrack if necessary.

4. **Skipping Newlines**: The function calls `skipNewlines()` to ensure that any whitespace or newlines following the operator are skipped.

5. **Checking Token Type**: The function checks if the next token is either `TokenType::AND` or `TokenType::AND_AND`. If neither is found, the parser backtracks to the saved position and breaks out of the loop.

6. **Consuming Operator Token**: If an AND operator is found, the function consumes it using `consume()`. The line number of the operator is recorded using `int ln = current().line;`. The string representation of the operator (`"and"` or `"&&"`) is determined based on the type of the consumed token.

7. **Parsing Right Hand Side**: After consuming the operator token, the function again calls `parseBitwise()` to parse the right-hand side of the AND operation. This result is stored in the variable `right`.

8. **Creating Binary Expression Node**: The function creates a new ASTNode object representing a binary expression. This node contains:
   - The operator string (`"and"` or `"&&"`).
   - A move-constructed unique pointer to the left-hand side (`std::move(left)`).
   - A move-constructed unique pointer to the right-hand side (`std::move(right)`).

9. **Updating Left Variable**: The newly created ASTNode becomes the new left-hand side of the next potential AND operation, allowing for nested AND expressions.

10. **Returning Result**: Once the loop exits, the function returns the final ASTNode representing the entire parsed expression.

## Edge Cases
- **No AND Operators**: If there are no AND or bitwise AND operators in the input stream, the function will simply return the result of the initial call to `parseBitwise()`.
- **Nested AND Expressions**: The function supports nested AND expressions, allowing for complex boolean logic to be represented accurately in the AST.
- **Whitespace and Newlines**: The function correctly handles whitespace and newlines between tokens, ensuring that they do not interfere with the parsing process.

## Interactions with Other Components
- **Tokenizer**: The `current()` and `consume()` functions rely on the tokenizer component to provide the next token in the input stream.
- **Error Handling**: Although not shown in this snippet, the function likely interacts with error handling mechanisms to report issues such as missing operands or unexpected token types.
- **Abstract Syntax Tree (AST)**: The function constructs and updates nodes in the AST, interacting with the ASTBuilder component to manage the structure of the parsed program.