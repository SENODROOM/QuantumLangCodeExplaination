# parseArrowFunction

## Purpose

The `parseArrowFunction` method is responsible for parsing arrow functions within the Quantum Language compiler. Arrow functions provide a concise syntax for defining anonymous functions, commonly used for tasks like creating callbacks or simple transformations.

## Parameters

- **params**: A vector of `Parameter` objects representing the parameters of the arrow function being parsed.
- **ln**: An integer indicating the line number where the arrow function begins.

## Return Value

The method returns a unique pointer to an `ASTNode` object, encapsulating the parsed arrow function. The returned node contains a `LambdaExpr` object, which holds the parameters and the body of the function.

## How It Works

1. **Token Matching**:
   - The method first checks if the current token is either `FAT_ARROW` (`=>`) or `ARROW` (`->`). If neither token is found, it throws a `ParseError`, indicating that an expected token was not encountered.

2. **Skipping Newlines**:
   - After matching the fat arrow or regular arrow token, the method calls `skipNewlines()` to ensure that any newlines following the token are skipped over.

3. **Parsing Function Body**:
   - The method then checks if the next token is either `LBRACE` (`{`) or `INDENT`. This indicates whether the function body consists of multiple statements enclosed in curly braces or a single statement.
     - If the body is a block, it calls `parseBlock()` to parse the block of statements and constructs a `LambdaExpr` object with these parameters and the parsed block.
     - If the body is a single expression, it parses the expression using `parseExpr()`. To make it compatible with the lambda syntax, it wraps the expression in an implicit return statement (`ReturnStmt`). This wrapped statement is then placed inside a block (`BlockStmt`). Finally, it constructs a `LambdaExpr` object with these parameters and the implicit return block.

4. **Returning the ASTNode**:
   - Regardless of whether the body was a block or a single expression, the method creates an `ASTNode` containing the constructed `LambdaExpr` and returns it.

## Edge Cases

- **Missing Fat Arrow or Regular Arrow Token**: If the parser encounters a token sequence that does not start with `=>` or `->`, it will throw a `ParseError`.
- **Empty Function Body**: While the method does not explicitly handle empty function bodies, the logic for parsing blocks or expressions ensures that the absence of content results in appropriate handling (e.g., an empty block or an expression resulting in `nullptr`).

## Interactions with Other Components

- **Tokenizer**: The method relies on the tokenizer to identify and return tokens such as `FAT_ARROW`, `ARROW`, `LBRACE`, and `INDENT`.
- **Expression Parser**: For single-expression bodies, the method utilizes the `parseExpr()` function to parse the expression into an AST node.
- **Block Parser**: When dealing with block bodies, the method invokes `parseBlock()` to parse the series of statements within the curly braces.

This method plays a crucial role in interpreting the quantum language's syntax for arrow functions, ensuring they are correctly translated into their corresponding abstract syntax tree nodes.