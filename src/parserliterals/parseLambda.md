# `parseLambda`

## Purpose

The `parseLambda` function in the Quantum Language compiler is responsible for parsing lambda expressions. It supports both anonymous and named forms of lambda expressions, capturing parameters, their types, default arguments, and the body of the lambda.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed as parameters, but relies on global state (`current()`, `match()` functions).
  
- **Return Value**:
  - Returns a unique pointer to an `ASTNode` containing the parsed lambda expression (`LambdaExpr`).

## How It Works

1. **Line Number Capture**: The line number where the lambda expression starts is captured using `current().line`.

2. **Default Arguments Initialization**: An empty vector `defaultArgs` is initialized to store any default arguments associated with the lambda's parameters.

3. **Parameter Types Initialization**: An empty vector `paramTypes` is initialized to store the types of the lambda's parameters.

4. **Parsing Parameter List**: The `parseParamList` function is called to parse the list of parameters. If a parameter type is provided, it is stored in `paramTypes`. Default arguments are collected into `defaultArgs`.

5. **Matching Colon**: The function then matches the colon token (`:`) which separates the parameters from the body in Python-style lambda expressions.

6. **Arrow Token Matching**: Depending on the syntax being used (JavaScript-like `=>` or Quantum-specific `->`), the appropriate arrow token is matched. This indicates the start of the lambda's body.

7. **Skipping Newlines**: Any newlines encountered after the arrow token are skipped using `skipNewlines()`.

8. **Body Parsing**: The `parseBlock` function is called to parse the body of the lambda expression.

9. **Lambda Expression Construction**: A `LambdaExpr` object is constructed with the parsed parameters, parameter types, default arguments, and body.

10. **Creating AST Node**: Finally, a unique pointer to an `ASTNode` containing the `LambdaExpr` is created and returned.

### Edge Cases

- **Syntax Variations**: Handles both JavaScript-like `=>` and Quantum-specific `->` syntaxes.
- **Default Arguments**: Supports lambda expressions with default arguments.
- **Parameter Types**: Allows specifying parameter types, enhancing type safety.
- **Empty Body**: Can handle lambda expressions without a body (though this might be unusual and could lead to errors depending on the context).

### Interactions with Other Components

- **Tokenizer**: Uses tokens consumed by the tokenizer to identify and parse different parts of the lambda expression.
- **Parse Param List**: Relies on `parseParamList` to extract parameters and their attributes.
- **Parse Block**: Utilizes `parseBlock` to parse the lambda's body, which can contain multiple statements or expressions.
- **Error Handling**: Integrates with error handling mechanisms to report issues during parsing if necessary.

This comprehensive approach ensures that the `parseLambda` function accurately captures all essential elements of a lambda expression, making it robust and versatile for various quantum programming scenarios.