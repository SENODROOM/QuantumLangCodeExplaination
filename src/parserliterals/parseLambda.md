# `parseLambda`

## Purpose

The `parseLambda` function in the Quantum Language compiler is designed to parse lambda expressions, which can be either anonymous or named. This function captures essential details such as the list of parameters, their respective types, any default arguments provided, and the body of the lambda expression. The parsed lambda expression is then encapsulated into an ASTNode and returned.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the function signature; however, it relies on global state (`current()`, `match()` functions) and external data structures (`defaultArgs`, `paramTypes`).

- **Return Value**:
  - Returns a unique pointer to an ASTNode containing the parsed lambda expression. The ASTNode holds a LambdaExpr object, which includes the parameters, parameter types, default arguments, and the body of the lambda.

## Edge Cases

1. **Anonymous Form**: If the lambda expression is defined using the `fn` or `function` keyword, the function will start parsing immediately after these keywords without expecting a name. It will then proceed to parse the parameters, types, defaults, and body.

2. **Named Form**: Although not shown in the provided code snippet, when dealing with named lambda expressions, the parser would typically expect a name before the parameters. For example, `let myLambda = (x: int) -> x + 1`.

3. **Default Arguments**: The function allows for the presence of default arguments. These are captured in the `defaultArgs` vector and associated with their corresponding parameters.

4. **Parameter Types**: Each parameter must have a type specified. The `paramTypes` vector ensures that every parameter has its type correctly recorded.

5. **Arrow Syntax Variations**: The lambda expression can use different arrow syntaxes depending on the language variant being compiled. The function supports variations like `->`, `=>`, and `Quantum ->`. It detects the correct syntax by matching the appropriate token.

6. **Skipping Newlines**: After encountering the arrow syntax, the function skips any newlines before proceeding to parse the body of the lambda, ensuring proper handling of formatting within the source code.

7. **Parsing Errors**: If the expected tokens are not found during parsing (e.g., missing colon, fat arrow, or newline), the function will raise an error indicating the issue at the current line number.

## Interactions with Other Components

- **Lexical Analyzer (`current()`, `match()`)**: The `parseLambda` function interacts with the lexical analyzer to consume tokens and check their types. It uses `current()` to get the current token and `match()` to ensure the correct sequence of tokens is encountered.

- **ASTBuilder (`std::make_unique<ASTNode>()`)**: Once the lambda expression is fully parsed, the function constructs an ASTNode using the `ASTNode` constructor. This node contains a LambdaExpr object, which represents the parsed lambda, along with the line number where it was defined.

- **Error Handling**: The function incorporates error handling mechanisms to manage unexpected situations during parsing. If a required token is not matched, it raises an error with the current line number, facilitating easier debugging and correction of the source code.

Overall, the `parseLambda` function plays a crucial role in accurately interpreting and converting lambda expressions into structured ASTNodes, enabling further processing and compilation steps within the Quantum Language compiler.