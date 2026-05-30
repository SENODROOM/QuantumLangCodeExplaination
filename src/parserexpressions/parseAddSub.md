# `parseAddSub`

## Function Overview

The `parseAddSub` function is an integral part of the Quantum Language compiler's parser, specifically tailored to manage the parsing of arithmetic expressions that incorporate both addition (`+`) and subtraction (`-`). This function effectively utilizes the functionalities offered by the parser to construct an Abstract Syntax Tree (AST) representing these expressions.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` object representing the parsed arithmetic expression. If there are any errors during parsing, the function will throw an exception.

### Why It Works This Way

The design of the `parseAddSub` function hinges on its ability to recursively parse sub-expressions using the `parseMulDiv` function, which handles multiplication, division, and unary operators. The primary reason for this approach is to ensure that the precedence rules of arithmetic operations are respected, particularly when dealing with mixed operations like addition and subtraction.

Here’s how the function works:
1. **Initial Parsing**: It begins by calling `parseMulDiv()` to parse the initial term or sub-expression of the arithmetic expression.
2. **Loop for Addition/Subtraction**: The function then enters a loop that continues as long as the next token in the input stream is either a plus sign (`+`) or a minus sign (`-`). Inside the loop:
   - It records the current line number (`ln`) where the operator is located.
   - It consumes the current token, obtaining the operator value (`op`).
   - It recursively calls `parseMulDiv()` again to parse the subsequent term or sub-expression.
   - It constructs a new `ASTNode` containing a `BinaryExpr` object, which represents the binary operation between the previously parsed term (`left`) and the newly parsed term (`right`). The constructor also takes the line number (`ln`) for error reporting purposes.
3. **Update Left Node**: The constructed `ASTNode` becomes the new `left` node for the next iteration of the loop, allowing the function to continue building the AST for the entire expression.
4. **Final Return**: Once the loop terminates (i.e., there are no more addition or subtraction operators), the function returns the final `ASTNode`, which now encapsulates the entire arithmetic expression.

### Edge Cases

- **Empty Expression**: If the input stream is empty or does not contain any valid tokens, the function will eventually exit the loop without constructing any nodes, leading to a return of `nullptr`.
- **Invalid Tokens**: If the input contains unexpected tokens (e.g., a string instead of a number), the `consume()` method will throw an exception, causing the function to terminate prematurely.
- **Mixed Precedence**: Although the function ensures correct precedence through recursion, it assumes that the input adheres to standard arithmetic rules. For instance, it does not handle parentheses explicitly; parentheses should be managed at a higher level of the parser to correctly group terms.

### Interactions With Other Components

- **Tokenizer**: The `parseAddSub` function relies on the tokenizer to provide the sequence of tokens. Each call to `current()` and `consume()` retrieves the next token from the tokenizer.
- **Error Handling**: During parsing, the function throws exceptions if it encounters invalid tokens or if the input stream is exhausted unexpectedly. These exceptions are propagated up to the caller, who can then handle them appropriately.
- **AST Construction**: After parsing each term and operation, the function constructs an AST using `std::make_unique<ASTNode>`. This AST is built incrementally, starting from the innermost expressions and working outward, ensuring that the structure accurately reflects the order of operations in the original source code.

In summary, the `parseAddSub` function is crucial for handling arithmetic expressions involving addition and subtraction in the Quantum Language compiler. Its recursive nature and reliance on `parseMulDiv` ensure that the correct precedence rules are applied, and it interacts seamlessly with the tokenizer and error handling mechanisms to produce a robust AST.