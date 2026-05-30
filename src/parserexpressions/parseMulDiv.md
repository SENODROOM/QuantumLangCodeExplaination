# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an essential component of the Quantum Language compiler's parser, specifically tailored to handle expressions that involve arithmetic operations such as multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures that these operations are correctly interpreted and parsed according to the language's syntax rules.

## Function Details

### Parameters

- None

### Return Value

- `std::unique_ptr<ASTNode>`: The function returns a unique pointer to an abstract syntax tree (AST) node representing the parsed expression. If parsing fails, it may return a null pointer or throw an exception.

### How It Works

1. **Initial Parsing**: The function starts by calling `parsePower()`, which is responsible for parsing expressions involving exponentiation (`**`) and parentheses. This initial call sets up the left-hand side (`left`) of the binary operation.

2. **Loop for Multiplicative and Divisive Operations**: 
   - The function enters a loop that continues as long as the next token matches any of the following types:
     - `TokenType::STAR`: Represents the multiplication operator (`*`).
     - `TokenType::SLASH`: Represents the division operator (`/`).
     - `TokenType::PERCENT`: Represents the modulo operator (`%`).
     - `TokenType::FLOOR_DIV`: Represents the floor division operator (`//`).

3. **Consuming Operator Token**: Inside the loop, the function consumes the current token using `consume()` and retrieves its value, which represents the arithmetic operator.

4. **Parsing Right-Hand Side**: After consuming the operator, the function calls `parsePower()` again to parse the right-hand side (`right`) of the binary operation.

5. **Constructing AST Node**: Once both sides are parsed, the function constructs a new AST node of type `BinaryExpr`. This node encapsulates the operator and the two operands (`left` and `right`). The line number (`ln`) associated with the current token is also stored in the AST node for debugging and error reporting purposes.

6. **Updating Left-Hand Side**: The newly constructed `BinaryExpr` node becomes the new left-hand side for potential further multiplicative or divisive operations, allowing the loop to continue.

7. **Return Statement**: After exiting the loop, the function returns the final left-hand side AST node, which now represents the entire parsed expression including all multiplicative and divisive operations.

### Edge Cases

- **Empty Expression**: If there are no tokens available for parsing, the function will likely return a null pointer or throw an exception, depending on how the parser is implemented.
  
- **Invalid Tokens**: If a token that is not one of the expected operators (`*`, `/`, `%`, `//`) is encountered, the loop condition will fail, and the function will return the initially parsed `left` node without any modifications.

- **Nested Expressions**: The function handles nested expressions due to the recursive nature of `parsePower()`, ensuring that the correct order of operations is respected.

### Interactions with Other Components

- **Lexer**: The `parseMulDiv` function relies on the lexer to provide the sequence of tokens for parsing. The lexer must be able to recognize and generate tokens for arithmetic operators and power expressions.

- **Error Handling**: The function may interact with the compiler's error handling mechanism to report syntax errors or invalid expressions. This could involve throwing exceptions or updating error logs.

- **AST Construction**: The function contributes to the construction of the AST by creating nodes that represent binary expressions. These nodes are then used by subsequent stages of the compilation process, such as semantic analysis and code generation.

In summary, the `parseMulDiv` function plays a crucial role in the Quantum Language compiler's parser by interpreting and constructing AST nodes for expressions involving multiplication, division, modulo, and floor division. Its design allows for the handling of complex expressions with multiple operations, respecting the precedence and associativity rules of these arithmetic operators.