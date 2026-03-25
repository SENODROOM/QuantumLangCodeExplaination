# `parseAddSub`

## Function Overview

The `parseAddSub` function is part of the Quantum Language compiler's parser and is responsible for parsing expressions involving addition (`+`) and subtraction (`-`). This function operates recursively to handle nested expressions and builds an Abstract Syntax Tree (AST) representing the parsed expression.

## Parameters and Return Value

### Parameters

- None explicitly listed in the provided code snippet; however, it relies on global state managed by the parser, such as the current token and the ability to consume tokens.

### Return Value

- `std::unique_ptr<ASTNode>`: The function returns a unique pointer to an AST node that represents the parsed expression. If there are no valid tokens or errors occur during parsing, it may return a null pointer.

## How It Works

1. **Initial Parsing**: The function starts by calling `parseMulDiv()`, which is presumably another method within the same class or namespace. This initial call parses the left-hand side of the expression, handling multiplication, division, and unary operators.

2. **Loop for Addition/Subtraction**: After obtaining the initial left-hand side (`left`), the function enters a loop that continues as long as the next token is either a plus (`TokenType::PLUS`) or minus (`TokenType::MINUS`) operator.

3. **Token Consumption and AST Construction**:
   - Inside the loop, the function records the current line number (`ln`) using `current().line`.
   - It then consumes the current token using `consume()` and extracts its value (`op`), which should be either `"+"` or `"`-"`.
   - Another call to `parseMulDiv()` is made to parse the right-hand side (`right`) of the current operation.
   - A new `ASTNode` is created using `std::make_unique`, encapsulating a `BinaryExpr` object. This object holds the operator (`op`), the left-hand side (`std::move(left)`), and the right-hand side (`std::move(right)`).
   - The new `ASTNode` becomes the new left-hand side for the next iteration of the loop, effectively chaining operations together.

4. **Exit Loop**: Once the loop exits (i.e., there are no more addition or subtraction operators), the function returns the final `ASTNode`.

## Edge Cases

- **Empty Input**: If the input stream is empty or contains no valid tokens, the function will likely return a null pointer or throw an exception, depending on how the parser handles errors.
- **Syntax Errors**: If the input contains unexpected tokens or incorrect syntax (e.g., missing operands after an operator), the function may fail to construct a valid AST or throw an error.
- **Nested Expressions**: The function correctly handles nested expressions by recursively calling itself when encountering multiplication/division operators. This ensures that operations are evaluated according to their precedence.

## Interactions with Other Components

- **Tokenizer**: The `parseAddSub` function relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()` and `consume()` to interact with the tokenizer's state.
- **Error Handling**: While not explicitly shown in the snippet, the parser likely has mechanisms to handle errors gracefully, possibly throwing exceptions or returning null pointers when syntax errors are detected.
- **Precedence Handling**: By calling `parseMulDiv()` inside the loop, the function implicitly respects the precedence of multiplication and division over addition and subtraction. This allows for correct evaluation of complex expressions without additional logic for operator precedence.

This recursive approach to parsing arithmetic expressions is a common pattern in compilers, ensuring that the structure of the expression is accurately represented in the AST and that operations are evaluated correctly based on their precedence.