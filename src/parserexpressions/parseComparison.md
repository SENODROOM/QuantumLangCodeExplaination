# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`).

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed comparison expression.

## How It Works
1. The function starts by parsing the left-hand side of the comparison using the `parseShift()` method.
2. It then enters a loop that continues as long as the next token is one of the comparison operators (`<`, `>`, `<=`, `>=`, `in`, `is`, `not`).
3. Inside the loop:
   - If the current token is `is`, the function checks if the next token is `not`. If so, it consumes both tokens and creates a `BinaryExpr` node with the value `"is not"`. Otherwise, it creates a `BinaryExpr` node with the value `"is"`.
   - If the current token is `not`, the function consumes it and checks if the next token is `in`. If so, it consumes both tokens and creates a `BinaryExpr` node with the value `"not in"`. If not, it throws a `ParseError` indicating that "Expected 'in' after 'not'".
   - If the current token is any of the other comparison operators (`<`, `>`, `<=`, `>=`), the function consumes the token and creates a `BinaryExpr` node with the corresponding operator.
4. After creating the `BinaryExpr` node, the function updates the line number (`ln`) to the current line and continues parsing the right-hand side of the comparison using `parseShift()`.
5. The loop ends when there are no more comparison operators, and the function returns the final `left` ASTNode.

## Edge Cases
- The function correctly handles the negation of the `in` and `is` operators (`not in`, `is not`).
- If the `not` operator is followed by a token other than `in` or `is`, a `ParseError` is thrown.
- The function assumes that the `parseShift()` method can handle the parsing of shift operations and other lower-precedence expressions.

## Interactions With Other Components
- This function interacts with the `Tokenizer` component to retrieve the next token during parsing.
- It utilizes the `consume()` method to advance the tokenizer's position to the next token after parsing each part of the comparison expression.
- The `match()` method is used to determine if the next token matches a specific type, such as `TokenType::IN` or `TokenType::NOT`.
- The `check()` method is used to verify if the current token matches a specific type before consuming it.
- The function constructs an `ASTNode` tree representing the parsed comparison expression, which is used by other parts of the compiler for further processing.