# `parseUnary`

The `parseUnary` function in the Quantum Language compiler is responsible for parsing unary expressions. These expressions consist of a single operator followed by an operand. The function handles various unary operators such as increment (`++`), decrement (`--`), negation (`-`), logical NOT (`!`), bitwise NOT (`~`), address-of (`&`), and dereference (`*`). It constructs an appropriate Abstract Syntax Tree (AST) node based on the parsed expression.

## What it Does

The primary role of `parseUnary` is to recognize and parse unary expressions from the input source code. It checks for different unary operators and constructs corresponding AST nodes. For example, it parses `++x` into an `AssignExpr` representing `x += 1`, and `-y` into a `UnaryExpr` representing `-y`.

## Why it Works This Way

This implementation ensures that each unary operator is correctly recognized and handled according to its semantics. By using recursive calls to `parseUnary`, it can handle nested unary expressions, such as `-(++x)` or `*(p++)`. The use of `std::make_unique` helps manage memory efficiently by creating unique pointers to AST nodes.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A `std::unique_ptr<ASTNode>` representing the parsed unary expression.

## Edge Cases

1. **Increment/Decrement Operators**: If the next token is either `++` or `--`, the function consumes the operator and recursively parses the operand. It then creates an `AssignExpr` with the appropriate assignment operation (`"+="` for `++` and `"-="` for `--`) and a literal value of `1.0`.
   
2. **Negation Operator**: If the next token is `-`, the function consumes the operator and recursively parses the operand. Since negation is a no-op in this context (i.e., `-1` is equivalent to `1`), it simply returns the result of the recursive call.

3. **Logical NOT Operator**: If the next token is `!`, the function consumes the operator and recursively parses the operand. It then creates a `UnaryExpr` with the operator `"not"`.

4. **Bitwise NOT Operator**: If the next token is `~`, the function consumes the operator and recursively parses the operand. It then creates a `UnaryExpr` with the operator `"~"`.

5. **Address-of Operator**: If the next token is `&`, the function consumes the operator. If the following token is an identifier containing `::`, it extracts the member name after `::` and returns a `StringLiteral` node. Otherwise, it recursively parses the operand and creates an `AddressOfExpr` node.

6. **Dereference Operator**: If the next token is `*`, the function consumes the operator and recursively parses the operand. It then creates a `DerefExpr` node.

## Interactions with Other Components

- **Token Stream**: The function interacts with the token stream to check and consume tokens. It uses helper functions like `current()`, `check()`, and `consume()` to manipulate the token stream.
  
- **Abstract Syntax Tree (AST)**: The function constructs AST nodes based on the parsed unary expressions. These nodes are used by subsequent stages of the compilation process, such as semantic analysis and code generation.

- **Error Handling**: While not explicitly shown in the provided snippet, the function should include error handling mechanisms to ensure that invalid unary expressions are detected and reported appropriately.

By understanding how `parseUnary` works, developers can better grasp the structure and behavior of unary expressions within the Quantum Language compiler, facilitating more accurate and efficient parsing of source code.