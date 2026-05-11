# `parseMulDiv`

## Function Purpose

The `parseMulDiv` function is an essential component of the Quantum Language compiler's parser, responsible for interpreting expressions that involve multiplication (`*`), division (`/`), modulo (`%`), and floor division (`//`). This function ensures that these arithmetic operations are correctly parsed and evaluated within the context of the quantum language syntax.

## Parameters

- None explicitly listed in the provided code snippet, but implicitly relies on global state or passed variables to determine the current token and its type.

## Return Value

- The function returns a unique pointer to an `ASTNode`, representing the root of the abstract syntax tree (AST) for the parsed expression. This AST node encapsulates a `BinaryExpr` object, which contains the operation type (`op`) and pointers to the left and right operands (`left` and `right`).

## How It Works

1. **Initialization**: The function begins by calling `parsePower()`, which parses the highest precedence expression (typically exponentiation). The result of this call is stored in the `left` variable.

2. **Loop for Multiplicative Operations**: A `while` loop checks if the next token is one of the multiplicative operators (`*`, `/`, `%`, `//`). If any of these operators are found, the loop proceeds:
   - **Line Number Retrieval**: The line number of the current token is retrieved using `current().line`.
   - **Operator Consumption**: The operator token is consumed using `consume().value`, storing the operator string in the `op` variable.
   - **Right Operand Parsing**: Another call to `parsePower()` is made to parse the right operand of the current operation. The result is stored in the `right` variable.
   - **AST Node Creation**: A new `ASTNode` is created, containing a `BinaryExpr` object initialized with the operator `op`, the previously parsed left operand `left`, and the newly parsed right operand `right`. The line number is also passed to the `ASTNode` constructor.
   - **Update Left Operand**: The `left` variable is updated to point to the newly created `ASTNode`, effectively chaining the parsed expressions together.

3. **Edge Cases**:
   - **Empty Expression**: If there are no tokens to parse or if the first token is not a valid operand, the function may return an empty `std::unique_ptr<ASTNode>`.
   - **Invalid Operator Sequence**: If consecutive invalid operators are encountered (e.g., `*/`), the function should handle this gracefully, possibly throwing an error or skipping the invalid sequence.
   - **Division by Zero**: While not directly handled by this function, the evaluation phase of the compiler should detect and handle division by zero errors appropriately.

4. **Interactions with Other Components**:
   - **Tokenizer**: The `parseMulDiv` function relies on the tokenizer to provide the next token for parsing. The tokenizer must be able to recognize and categorize tokens such as numbers, identifiers, and operators.
   - **Error Handling**: During the parsing process, the function may encounter unexpected tokens or sequences. Error handling mechanisms, such as exceptions or error codes, should be used to manage these situations gracefully.
   - **Precedence Handling**: The function uses a loop to handle multiple multiplicative operations in sequence. However, for full support of operator precedence, additional functions or mechanisms would need to be integrated to handle higher precedence operations like addition and subtraction.

Overall, the `parseMulDiv` function plays a crucial role in parsing complex arithmetic expressions in the Quantum Language. Its design allows for easy extension to support additional arithmetic operations and helps maintain a clear separation between lexical analysis and syntactic analysis in the compiler.