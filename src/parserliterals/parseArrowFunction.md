# parseArrowFunction

## Purpose

The `parseArrowFunction` method is designed to handle the parsing of arrow functions within the Quantum Language compiler. Arrow functions provide a more concise syntax compared to traditional function declarations, making them ideal for quick and simple operations such as mapping over lists or performing calculations on quantum states.

## Parameters

- `params`: A vector of `Parameter` objects representing the parameters of the arrow function being parsed.
- `ln`: An integer representing the line number where the arrow function starts.

## Return Value

The method returns a unique pointer to an `ASTNode`, which represents the parsed arrow function. The `ASTNode` contains a `LambdaExpr` object, which holds the parameters and body of the arrow function.

## How It Works

1. **Token Matching**: The method first checks if the current token is either `TokenType::FAT_ARROW` (i.e., `=>`) or `TokenType::ARROW` (i.e., `->`). If neither token is found, a `ParseError` is thrown indicating that the expected tokens were not found at the specified line and column.

2. **Skipping Newlines**: After matching the arrow token, the method calls `skipNewlines()` to ensure that any newlines following the arrow token are skipped, allowing for better readability and handling of multi-line arrow functions.

3. **Body Parsing**:
   - If the next token is either `TokenType::LBRACE` (indicating a block body) or `TokenType::INDENT` (indicating an implicitly indented block body), the method parses the block using `parseBlock()`. This block is then wrapped into a `LambdaExpr` object along with the previously parsed parameters.
   
   - If the next token is not a block indicator, the method assumes that the body is a single expression. It parses this expression using `parseExpr()`.

4. **Implicit Return Handling**:
   - For single-expression bodies, the method wraps the parsed expression inside a `ReturnStmt` object. This `ReturnStmt` is then placed within a `BlockStmt`, which is itself wrapped inside another `ASTNode`.
   - Finally, the `LambdaExpr` containing the parameters, the implicitly returned block, and the original line number is wrapped in an `ASTNode` and returned.

5. **Edge Cases**:
   - The method handles both block and single-expression arrow function bodies, ensuring flexibility in syntax.
   - It correctly identifies and handles implicit return blocks by wrapping single expressions inside a return statement and then inside a block.

6. **Interactions with Other Components**:
   - `parseArrowFunction` interacts with the `Tokenizer` class through methods like `match()` and `check()` to identify and consume tokens during parsing.
   - It also uses the `Parser` class's `parseBlock()` and `parseExpr()` methods to further break down the body of the arrow function into its constituent parts.
   - The resulting `ASTNode` is used by other components of the compiler to generate code or perform semantic analysis.

This method effectively parses arrow functions by recognizing their syntax and handling both block and single-expression forms, thereby facilitating the compilation process in the Quantum Language compiler.