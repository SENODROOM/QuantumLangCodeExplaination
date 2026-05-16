# `parseLambda`

## Purpose

The `parseLambda` function is designed to parse a lambda expression within the Quantum Language compiler. This function supports both anonymous and named forms of lambda expressions, handling the capture of parameters, their types, default arguments, and the body of the lambda.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet. However, it is implied that the function relies on global state or context variables such as `current()` which likely provides information about the current token being processed.

- **Return Value**:
  - The function returns a unique pointer to an `ASTNode` object containing a parsed `LambdaExpr`. The `LambdaExpr` includes details like parameters (`params`), parameter types (`paramTypes`), default arguments (`defaultArgs`), and the body of the lambda (`body`). Each of these members is moved into the `LambdaExpr` to ensure efficient memory management.

## How It Works

1. **Capture Current Line Number**: The function starts by capturing the current line number using `current().line`. This line number is stored in the `ln` variable and will be used later when creating the `ASTNode`.

2. **Initialize Default Arguments and Parameter Types Vectors**: Two vectors, `defaultArgs` and `paramTypes`, are initialized to store the default arguments and types of the parameters respectively.

3. **Parse Parameter List**: The function calls `parseParamList(nullptr, &defaultArgs, &paramTypes)` to parse the list of parameters. This method likely handles the syntax for defining parameters, including their names, types, and any default values. The result is stored in the `params` vector.

4. **Match Colon Token**: The function then matches the colon token (`:`) which separates the parameters from the body of the lambda. This step ensures that the syntax is correctly followed according to the language rules.

5. **Determine Arrow Type**: Depending on whether the next token is a fat arrow (`->`) or a regular arrow (`=>`), the function proceeds accordingly. Both symbols are commonly used in lambda expressions to denote the transition between the parameters and the body of the function.

6. **Skip Newlines**: After determining the type of arrow, the function skips any newline characters using `skipNewlines()`. This helps in parsing multiline lambda bodies more gracefully.

7. **Parse Body Block**: The function calls `parseBlock()` to parse the body of the lambda expression. The result is stored in the `body` variable.

8. **Construct Lambda Expression Object**: A `LambdaExpr` object is constructed and populated with the parsed parameters, parameter types, default arguments, and body.

9. **Create ASTNode and Return**: Finally, the function creates an `ASTNode` object, moves the `LambdaExpr` into it, and returns a unique pointer to this node along with the captured line number.

## Edge Cases

- **Missing Arrow**: If neither the fat arrow (`->`) nor the regular arrow (`=>`) is found after the colon, the function defaults to matching the regular arrow (`=>`). This ensures compatibility with various lambda syntaxes.
  
- **Empty Parameters**: If there are no parameters defined, the `parseParamList` method should handle this case appropriately, returning empty vectors for `params` and `paramTypes`.
  
- **Multiline Body**: The `skipNewlines()` call allows for parsing lambda expressions with bodies spread across multiple lines, making the parser more robust.

## Interactions With Other Components

- **Parsing Context**: The function relies on the current parsing context, which might include methods like `current()`, `match()`, and `skipNewlines()`. These methods help in navigating through the tokens and ensuring correct syntax matching.

- **Abstract Syntax Tree (AST)**: The function constructs an `ASTNode` containing a `LambdaExpr`. This interaction with the AST is crucial for representing the parsed lambda expression in a structured format that can be further analyzed or executed by the compiler.

- **Error Handling**: While not explicitly shown in the provided code snippet, typical implementations of such functions would include error handling mechanisms to manage unexpected tokens or syntax errors during parsing.

This comprehensive approach ensures that the lambda expression is accurately parsed and represented within the Quantum Language compiler's abstract syntax tree, facilitating subsequent compilation steps.