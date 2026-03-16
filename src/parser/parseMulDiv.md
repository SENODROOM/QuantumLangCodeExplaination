# `parseMulDiv` Function

## Purpose
The `parseMulDiv` function is designed to parse expressions involving multiplication (`*`), division (`/`), modulus (`%`), and floor division (`//`). It builds an Abstract Syntax Tree (AST) node representing these operations based on the input tokens provided by the parser.

## Parameters
- None explicitly mentioned in the code snippet.

## Return Value
- Returns a unique pointer to an ASTNode object that represents the parsed expression. The ASTNode contains either a BinaryExpr or another type of expression depending on the context.

## How It Works
1. **Initial Parsing**: The function starts by calling `parsePower()`, which is presumably responsible for parsing expressions involving exponentiation. This initial call sets up the left-hand side of the binary operation.

2. **Loop for Multiplicative Operations**: The function enters a loop that continues as long as the next token is one of the following types:
   - TokenType::STAR (`*`) for multiplication.
   - TokenType::SLASH (`/`) for division.
   - TokenType::PERCENT (`%`) for modulus.
   - TokenType::FLOOR_DIV (`//`) for floor division.

3. **Consuming Operator Token**: Inside the loop, the function consumes the current token using `consume()` and retrieves its value. This value is used as the operator in the binary expression.

4. **Recursive Parsing**: After consuming the operator token, the function calls `parsePower()` again to parse the right-hand side of the binary operation. This recursive call ensures that nested expressions can be correctly handled.

5. **Building AST Node**: Once both operands are parsed, the function creates a new ASTNode containing a BinaryExpr. The constructor of BinaryExpr takes three arguments:
   - The operator string.
   - A move-constructed unique pointer to the left-hand side ASTNode.
   - A move-constructed unique pointer to the right-hand side ASTNode.
   - The line number where the operator was found, obtained using `current().line`.

6. **Updating Left Node**: The newly created ASTNode becomes the new left-hand side for the next iteration of the loop, allowing for the construction of complex expressions involving multiple multiplicative operations.

7. **Loop Termination**: The loop terminates when there are no more tokens of the specified types, meaning all multiplicative operations have been processed.

8. **Return Statement**: Finally, the function returns the left-hand side ASTNode, which now represents the entire parsed expression including all multiplicative operations.

## Edge Cases
- **Empty Expression**: If the input stream is empty or does not contain any valid tokens, the function will eventually exit the loop without creating any AST nodes, returning the result of `parsePower()`.
- **Invalid Tokens**: If the next token is not one of the expected types (`*`, `/`, `%`, `//`), the loop condition will fail, and the function will return the current left-hand side ASTNode.
- **Nested Expressions**: The function handles nested expressions through recursion, ensuring that the correct precedence of operations is maintained.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. The tokenizer must correctly identify and categorize tokens such as operators and operands.
- **Error Handling**: While not shown in the snippet, the function likely interacts with error handling mechanisms to report syntax errors if unexpected tokens are encountered during parsing.
- **Precedence Handling**: The use of a loop allows the function to handle multiple consecutive multiplicative operations, respecting their order according to the rules of arithmetic precedence.

This function is crucial for constructing the AST for expressions involving multiplication and related operations, enabling further analysis and evaluation of the quantum language code.