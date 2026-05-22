# `parseComparison` Function

## Purpose
The `parseComparison` function is designed to parse comparison expressions in the Quantum Language compiler. It handles various types of comparisons including `<`, `>`, `<=`, `>=`, `in`, and `is` operators, as well as their negated forms (`not in`, `is not`). This function ensures that the parsed expressions are correctly structured into an Abstract Syntax Tree (AST).

## Parameters
- **None**: The function does not take any explicit parameters. It operates on the global parser state, accessing the current token and consuming tokens as necessary.

## Return Value
- **std::unique_ptr<ASTNode>**: The function returns a unique pointer to an ASTNode representing the parsed comparison expression. If no valid comparison operator is found or an error occurs during parsing, the function will throw a `ParseError`.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseShift()` to parse the left-hand side (LHS) of the comparison expression. This LHS can be any expression that is valid according to the grammar rules defined elsewhere in the compiler.

2. **Loop for Comparison Operators**: The function enters a loop that continues as long as the next token is one of the comparison operators (`<`, `>`, `<=`, `>=`, `in`, `is`, `not in`, `is not`). Inside the loop:
   - **Line Number Retrieval**: The line number of the current token is retrieved using `current().line`.
   - **Negated Forms Handling**:
     - If the current token is `is`, the function checks if the next token is `not`. If so, it consumes both tokens and parses the right-hand side (RHS). The resulting ASTNode represents the `is not` operation.
     - If the current token is `is not`, it directly parses the RHS and constructs an ASTNode representing the `is not` operation.
     - For the `not in` operator, the function consumes the `not` token, then checks if the next token is `in`. If not, it throws a `ParseError`. After confirming `in`, it parses the RHS and constructs an ASTNode representing the `not in` operation.
   - **Standard Comparisons**: For standard comparison operators (`<`, `>`, `<=`, `>=`), the function consumes the operator token, parses the RHS, and constructs an ASTNode representing the corresponding binary expression.

3. **Return Statement**: Once the loop exits (i.e., no more comparison operators are found), the function returns the unique pointer to the ASTNode representing the entire comparison expression.

## Edge Cases
- **Invalid Negation**: If a `not` token is encountered but followed by a non-`in` or non-`is` token, the function throws a `ParseError` indicating that "Expected 'in' after 'not'".
- **Empty Expression**: If there are no valid comparison operators following the initial LHS, the function returns the LHS node as is, without constructing a comparison expression.
- **Nested Comparisons**: The function supports nested comparisons, meaning you can have expressions like `a < b <= c` which would be parsed correctly.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()`, `consume()`, and `match()` to interact with the tokenizer.
- **AST Construction**: The function constructs an AST using the `ASTNode` class and `BinaryExpr` struct. These classes and structs are part of the compiler's internal data structures used to represent the parsed code.
- **Error Handling**: The function throws a `ParseError` when encountering unexpected tokens or syntax errors, ensuring that the compiler can gracefully handle and report issues to the user.

Overall, `parseComparison` plays a crucial role in the Quantum Language compiler by accurately parsing complex comparison expressions and building them into the AST, enabling further compilation steps such as semantic analysis and code generation.