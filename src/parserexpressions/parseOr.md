# `parseOr` Function

## Purpose
The `parseOr` function is crucial to the parser within the Quantum Language compiler. Its primary role is to interpret expressions involving the logical OR (`or`), logical AND (`&&`), and null-coalescing (`??`) operators. By constructing an Abstract Syntax Tree (AST), this function ensures that these complex expressions are correctly parsed and represented in a structured format.

## Parameters
- None

## Return Value
- A unique pointer to an `ASTNode`, representing the root of the parsed expression tree.

## How It Works
The `parseOr` function begins by parsing the left-hand side of the expression using the `parseAnd()` method. This initial call sets up the starting point for the binary expression tree. The function then enters a loop where it repeatedly checks for subsequent OR, AND, or null-coalescing operators.

1. **Initialization**: 
   - `auto left = parseAnd();`: Parses the left-hand side of the expression using `parseAnd()`.
   
2. **Loop**:
   - The loop continues as long as one of the following tokens is found: `TokenType::OR`, `TokenType::OR_OR`, or `TokenType::NULL_COALESCE`.
   - Inside the loop, `size_t savedPos = pos;` saves the current position in the token stream to allow backtracking if necessary.
   - `skipNewlines();` skips any newline characters to ensure proper parsing of the next token.
   - `if (!check(TokenType::OR) && !check(TokenType::OR_OR) && !check(TokenType::NULL_COALESCE)) { ... }`: Checks if the current token is not one of the expected OR, AND, or null-coalescing operators. If not, it restores the saved position (`pos = savedPos;`) and breaks out of the loop.
   - `int ln = current().line;`: Retrieves the line number of the current token for error reporting purposes.
   - `auto opToken = consume();`: Consumes the current token, which could be either `TokenType::OR`, `TokenType::OR_OR`, or `TokenType::NULL_COALESCE`. Depending on its type, it assigns the string representation of the operator to `opStr`.
   - `skipNewlines();` again skips any newline characters after consuming the operator token.
   - `auto right = parseAnd();`: Parses the right-hand side of the current binary expression using `parseAnd()`.
   - `left = std::make_unique<ASTNode>(BinaryExpr{opStr, std::move(left), std::move(right)}, ln);`: Constructs a new `ASTNode` containing a `BinaryExpr` object, which represents the binary operation. The `BinaryExpr` takes the operator string (`opStr`), the previously parsed left-hand side (`std::move(left)`), and the newly parsed right-hand side (`std::move(right)`). The line number (`ln`) is also stored in the `ASTNode`.

3. **Completion**:
   - Once the loop exits, the function returns the final `ASTNode`, which now contains the entire parsed expression tree.

## Edge Cases
- **Empty Expression**: If there are no tokens to parse, the function will simply return the result of `parseAnd()`, which may be a null pointer or an empty node depending on the implementation.
- **No OR, AND, or Null-Coalescing Operators**: If the expression does not contain any of the specified operators, the function will return the result of `parseAnd()` without modifying it.
- **Nested Expressions**: The function can handle nested expressions involving multiple OR, AND, and null-coalescing operators. Each nested expression is parsed recursively until all tokens have been consumed.

## Interactions With Other Components
- **Tokenizer**: The `parseOr` function relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()` and `consume()` to interact with the tokenizer.
- **Error Handling**: The function includes calls to `skipNewlines()` to manage whitespace and potential errors gracefully. For example, if an unexpected token is encountered, the function can backtrack and continue parsing.
- **Expression Parsing**: The `parseOr` function builds upon the functionality provided by `parseAnd()`, which handles lower precedence operations. Together, they form a robust system for parsing quantum language expressions.

This comprehensive approach ensures that the `parseOr` function can accurately interpret and represent complex quantum language expressions in the AST, facilitating further processing during compilation.