# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is part of the Quantum Language compiler's parser and is responsible for parsing expressions involving multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures that these operations are correctly handled in the abstract syntax tree (AST) during the compilation process.

## Why It Works This Way

The function uses a loop to repeatedly parse multiplication, division, modulo, and floor division operations after parsing a power expression (`parsePower()`). This design allows for handling multiple consecutive arithmetic operations within an expression, ensuring correct precedence and associativity rules are respected.

- **Loop Structure**: The loop continues as long as any of the specified token types (`TokenType::STAR`, `TokenType::SLASH`, `TokenType::PERCENT`, `TokenType::FLOOR_DIV`) are found in the input stream.
- **Precedence Handling**: By using a loop, the function can handle higher precedence operations first (e.g., exponentiation before multiplication).
- **Associativity Handling**: Left associativity is naturally enforced by the order in which operations are parsed and combined.

## Parameters/Return Value

### Parameters

- None explicitly listed in the provided code snippet.

### Return Value

- `std::unique_ptr<ASTNode>`: A unique pointer to the root node of the AST representing the parsed expression. If there are no multiplicative or division operations, it returns the result of `parsePower()`.

## Edge Cases

1. **No Operations**: If the input expression contains only one term without any multiplication, division, modulo, or floor division operators, the function will simply call `parsePower()` and return its result.
2. **Mixed Precedence**: The function correctly handles mixed precedence scenarios by evaluating higher precedence operations first. For example, in the expression `2 * 3 + 4`, it will first evaluate `2 * 3` and then add `4`.
3. **Empty Input**: While not explicitly shown in the code, if the input stream is empty or does not contain any valid tokens, the function should gracefully handle this case, possibly returning a default AST node or throwing an appropriate error.

## Interactions with Other Components

- **ParserExpressions.h**: This header file likely contains declarations for related functions such as `parsePower()`, which `parseMulDiv()` calls to get the initial term of the expression.
- **TokenStream**: The function interacts with the `TokenStream` class to consume tokens and check their type. It uses methods like `current()`, `consume()`, and `check()` to manage the parsing process.
- **ASTNode**: This class represents nodes in the abstract syntax tree. `parseMulDiv()` constructs new `BinaryExpr` nodes to represent each operation and combines them into the final AST structure.

In summary, `parseMulDiv` is a crucial component of the Quantum Language compiler's parser, designed to handle complex arithmetic expressions involving multiple operations. Its implementation leverages a loop to ensure correct precedence and associativity, making it robust and versatile for various input scenarios.