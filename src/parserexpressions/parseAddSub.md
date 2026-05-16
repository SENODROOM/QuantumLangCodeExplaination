# `parseAddSub`

## Function Overview

The `parseAddSub` function is an essential part of the Quantum Language compiler's parser, responsible for handling arithmetic expressions involving addition (`+`) and subtraction (`-`). It serves as a higher-level parser that builds upon the lower-level `parseMulDiv` function to construct binary expression trees representing these operations.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: A `std::unique_ptr<ASTNode>` representing the parsed binary expression tree.

## How It Works

1. **Initialization**:
   - The function starts by calling `parseMulDiv()` to parse the left-hand side of the expression. This function returns a `std::unique_ptr<ASTNode>` representing the result of multiplication or division.

2. **Loop Through Operators**:
   - The function enters a loop that continues as long as the next token is either a plus (`TokenType::PLUS`) or minus (`TokenType::MINUS`) operator.
   
3. **Consume Operator**:
   - Inside the loop, the function consumes the current token (either `+` or `-`) using the `consume()` method. This method advances the parser to the next token and returns the consumed token.
   
4. **Parse Right Hand Side**:
   - After consuming the operator, the function again calls `parseMulDiv()` to parse the right-hand side of the expression. This step ensures that the right-hand side can also be a complex expression involving multiplication and division.
   
5. **Build Binary Expression Tree**:
   - With the left-hand side, the operator, and the right-hand side all parsed, the function constructs a new `ASTNode` containing a `BinaryExpr` object. The `BinaryExpr` object holds the operator, the left-hand side node, and the right-hand side node.
   - The line number (`ln`) associated with the current token is stored in the `BinaryExpr` to maintain accurate source code location information.
   - The newly constructed `ASTNode` is then assigned back to `left`, effectively updating the left-hand side of the expression tree for the next iteration of the loop.

6. **Repeat Until No More Operators**:
   - Steps 3 through 5 repeat until there are no more plus or minus operators in the input stream.

7. **Return Final AST Node**:
   - Once the loop completes, the function returns the final `std::unique_ptr<ASTNode>`. This node represents the entire arithmetic expression, including nested additions and subtractions, as a single binary expression tree.

## Edge Cases

- **Empty Input**: If the input stream is empty or contains only non-arithmetic tokens, the function will eventually exit the loop without constructing any nodes, returning `nullptr`.
- **Single Token**: If the input stream consists of a single token that is not an arithmetic operator, the function will return a `std::unique_ptr<ASTNode>` containing the result of parsing that single token.
- **Nested Expressions**: The function handles nested expressions correctly by recursively calling itself within the loop. For example, the expression `a + b * c - d` would be parsed into a tree structure reflecting the order of operations.

## Interactions with Other Components

- **Tokenizer**: The `parseAddSub` function relies on the tokenizer to provide the sequence of tokens. Each call to `current()` and `consume()` retrieves tokens from the tokenizer.
  
- **Lower-Level Parser (`parseMulDiv`)**: The `parseAddSub` function uses the `parseMulDiv` function to parse the operands of the addition and subtraction operations. This allows it to handle expressions involving multiplication and division before combining them with the add/subtract operators.

- **Error Handling**: While not explicitly shown in the provided code snippet, the `parseAddSub` function should integrate with the error handling mechanism of the compiler. This means that if any errors occur during the parsing process, such as unexpected token types or missing operands, appropriate error messages should be generated and handled.

By leveraging the `parseMulDiv` function and efficiently managing the recursive parsing of nested expressions, the `parseAddSub` function provides a robust solution for handling arithmetic expressions involving addition and subtraction in the Quantum Language compiler.