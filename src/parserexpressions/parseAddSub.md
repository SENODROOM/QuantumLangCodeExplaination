# `parseAddSub`

## Function Overview

The `parseAddSub` function is an integral component of the Quantum Language compiler's parser, tasked with interpreting arithmetic expressions that include both addition (`+`) and subtraction (`-`). This function operates at a higher level than its predecessor, `parseMulDiv`, which handles multiplication and division operations. The primary role of `parseAddSub` is to construct an Abstract Syntax Tree (AST) representing these expressions accurately.

### Parameters/Return Value

**Parameters:**
- None explicitly stated in the provided code snippet; however, it implicitly depends on global state managed by the parser, such as the current token being processed.

**Return Value:**
- Returns a unique pointer to an `ASTNode` object, encapsulating the parsed binary expression. If there are no valid tokens or errors occur during parsing, it may return `nullptr`.

### Edge Cases

1. **Empty Expression:** If there are no tokens available for parsing, the function should gracefully handle this case without crashing.
2. **Single Operand:** If only one operand is present (e.g., `5 +`), the function should throw an error indicating incomplete syntax.
3. **Mixed Operators:** The function should correctly handle expressions with mixed operators (e.g., `5 + 3 - 2 * 4`), respecting the precedence rules of arithmetic operations.
4. **Nested Expressions:** The function must be able to parse nested expressions (e.g., `(5 + 3) - 2`).

### Interactions with Other Components

1. **Tokenizer:** `parseAddSub` relies on the tokenizer to provide the sequence of tokens for parsing. The tokenizer breaks down the source code into individual tokens, which `parseAddSub` then uses to construct the AST.
2. **Error Handling:** During parsing, `parseAddSub` interacts with the error handler to report any syntax errors encountered. If an invalid token is found or the expression is incomplete, the error handler will generate appropriate error messages.
3. **Lower-Level Parser (`parseMulDiv`):** `parseAddSub` calls `parseMulDiv` to parse the operands of the binary expression. This allows `parseAddSub` to focus solely on constructing the AST for addition and subtraction, leveraging the existing functionality for multiplication and division.

### Why It Works This Way

The design of `parseAddSub` follows a recursive descent approach, where each function parses a specific type of expression. By calling `parseMulDiv()` within a loop, `parseAddSub` can repeatedly parse pairs of operands and operators until no more addition or subtraction operators are found. This ensures that all parts of the expression are correctly grouped according to their precedence.

For example, consider the expression `5 + 3 - 2 * 4`. The loop in `parseAddSub` would first parse `5 + 3`, resulting in a binary expression node. Then, it would parse `- 2 * 4`, again creating a binary expression node. Finally, it would combine these two nodes into a single binary expression node representing the entire original expression.

This method allows `parseAddSub` to handle complex arithmetic expressions efficiently, ensuring that the resulting AST accurately reflects the structure and intent of the source code.