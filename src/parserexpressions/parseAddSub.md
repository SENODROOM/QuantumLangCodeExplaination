# `parseAddSub`

## Function Overview

The `parseAddSub` function is a crucial component of the Quantum Language compiler's parser, specifically designed to handle arithmetic expressions that involve both addition (`+`) and subtraction (`-`). This function plays a pivotal role in breaking down complex expressions into simpler binary operations, allowing for accurate syntax analysis and semantic interpretation during compilation.

### Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet, but implicitly takes tokens from the tokenizer.
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed expression tree. The returned node encapsulates either a multiplication/division operation or a binary expression combining addition/subtraction with previously parsed nodes.

### How It Works

1. **Initialization**: The function begins by calling `parseMulDiv()`, which is assumed to be another method within the same class or namespace, responsible for parsing expressions involving multiplication (`*`), division (`/`), and unary operators. This initial call sets up the left-hand side of the binary expression.

2. **Loop for Addition/Subtraction**: The function then enters a loop that continues as long as the next token is either an addition (`TokenType::PLUS`) or a subtraction (`TokenType::MINUS`). Inside the loop:
   - **Line Number Retrieval**: The line number of the current token is stored in `ln`.
   - **Operator Consumption**: The operator token (`+` or `-`) is consumed using `consume()`, and its value is retrieved.
   - **Right Hand Side Parsing**: Another call to `parseMulDiv()` is made to parse the expression on the right-hand side of the current operator.
   - **Building AST Node**: A new `ASTNode` is constructed, wrapping a `BinaryExpr` object that holds the operator, the left-hand side node, and the right-hand side node. This new node becomes the new left-hand side for the next iteration of the loop.

3. **Termination**: Once the loop terminates (i.e., there are no more addition or subtraction operators), the function returns the final `ASTNode` built during the loop.

### Edge Cases

- **Empty Expression**: If `parseMulDiv()` returns a null pointer or an empty node at any point, the loop will terminate immediately, returning the initial left-hand side node.
- **Mixed Operators**: The function handles mixed sequences of addition and subtraction correctly, building a nested structure of binary expressions based on the order of operators encountered.
- **Whitespace and Comments**: While not directly shown in the code, the parser should ideally skip over whitespace and comments between tokens to ensure robustness.

### Interactions with Other Components

- **Tokenizer**: The `parseAddSub` function relies on the tokenizer to provide the sequence of tokens for parsing. Tokens such as numbers, identifiers, and operators are consumed and processed sequentially.
- **Error Handling**: Although not explicitly shown here, the parser likely includes error handling mechanisms to manage unexpected token types or malformed expressions gracefully.
- **Abstract Syntax Tree (AST)**: The function constructs an AST, where each node represents an operation or operand. This AST is used for further processing, including semantic analysis and code generation.

In summary, the `parseAddSub` function is essential for parsing arithmetic expressions involving addition and subtraction in the Quantum Language compiler. By leveraging recursive calls to `parseMulDiv()`, it efficiently handles nested expressions and builds a structured AST representation, enabling subsequent stages of compilation to proceed accurately and effectively.