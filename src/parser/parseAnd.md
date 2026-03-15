# `parseAnd`

## Purpose

The `parseAnd` function is part of the parser component in the Quantum Language compiler. Its primary purpose is to parse expressions involving the logical AND operator (`and`) and its shorthand form (`&&`). The function constructs an abstract syntax tree (AST) node representing these binary expressions.

## Functionality

1. **Initial Parsing**: The function begins by calling `parseBitwise()` to parse the left-hand side (LHS) of the expression. This LHS can be any bitwise operation or expression that precedes the AND operator.

2. **Loop for Multiple ANDs**: 
   - It enters a loop where it repeatedly checks for the presence of either `TokenType::AND` or `TokenType::AND_AND`.
   - If such an operator is found, it consumes the token and updates the line number (`ln`).
   - Depending on whether `TokenType::AND_AND` was consumed, it sets the operator string (`opStr`) to either `"and"` or the actual token value.
   - It then skips any newlines using `skipNewlines()`.
   - Another call to `parseBitwise()` parses the right-hand side (RHS) of the current AND operation.
   - A new `ASTNode` is created with a `BinaryExpr` containing the operator string, the LHS, and the RHS. This node represents the current AND expression.
   - The newly created node becomes the new LHS for the next iteration of the loop.

3. **Exit Loop**: The loop continues until no more AND operators are found. At this point, the function breaks out of the loop and returns the final AST node constructed.

## Parameters and Return Value

- **Parameters**:
  - None explicitly defined within the function signature; however, it relies on global state managed by the parser, such as the current position (`pos`) and the ability to check and consume tokens.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` representing the parsed AND expression(s). If no AND operators are found, it simply returns the initial LHS node.

## Edge Cases

- **No AND Operators**: If there are no `TokenType::AND` or `TokenType::AND_AND` operators in the input stream after the initial call to `parseBitwise()`, the function will return the single parsed node without constructing any additional nodes.
  
- **Nested AND Expressions**: The function correctly handles nested AND expressions by recursively building the AST. Each AND operation forms a subtree under the parent node.

- **Trailing Newlines**: The use of `skipNewlines()` ensures that any trailing newlines between AND operators do not interfere with parsing.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to check and consume tokens. The tokenizer must provide tokens of types `TokenType::AND` and `TokenType::AND_AND`.

- **Abstract Syntax Tree (AST)**: The function constructs `ASTNode` objects to represent the parsed expressions. These nodes are used by subsequent stages of the compiler to perform semantic analysis and code generation.

- **Error Handling**: While not shown in the snippet, the function likely interacts with error handling mechanisms to report errors related to unexpected tokens or malformed expressions.

In summary, the `parseAnd` function efficiently parses multiple AND operations into a structured AST, leveraging the tokenizer's capabilities and ensuring correct handling of various edge cases.