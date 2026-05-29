# `parseLambda`

## Purpose

The `parseLambda` function in the Quantum Language compiler is designed to parse lambda expressions, supporting both anonymous and named forms. This function captures essential details such as parameters, their types, default arguments, and the body of the lambda expression. The primary goal is to accurately convert the parsed lambda syntax into an Abstract Syntax Tree (AST) node that can be further processed by the compiler.

## Parameters/Return Value

### Parameters

- **None**: The function does not explicitly take any parameters directly. Instead, it relies on global state managed by the parser, including the current token being processed (`current()`).

### Return Value

- **ASTNodePtr**: The function returns a unique pointer to an ASTNode containing a LambdaExpr object. This ASTNode represents the parsed lambda expression and includes all necessary information about its structure and content.

## How It Works

1. **Line Number Capture**:
   ```cpp
   int ln = current().line;
   ```
   The line number where the lambda expression starts is captured using the `current()` function, which provides access to the current token's location.

2. **Parsing Default Arguments**:
   ```cpp
   std::vector<ASTNodePtr> defaultArgs;
   ```
   A vector to store default argument nodes is initialized. These nodes will represent the default values assigned to parameters within the lambda expression.

3. **Parsing Parameter Types**:
   ```cpp
   std::vector<std::string> paramTypes;
   ```
   Another vector is used to store parameter type strings. Each string in this vector corresponds to the type of a parameter in the lambda expression.

4. **Parsing Parameter List**:
   ```cpp
   auto params = parseParamList(nullptr, &defaultArgs, &paramTypes);
   ```
   The `parseParamList` function is called to parse the list of parameters. This function updates the `defaultArgs` and `paramTypes` vectors based on the parsed parameters.

5. **Matching Colon**:
   ```cpp
   match(TokenType::COLON);
   ```
   The parser expects a colon (`:`) to follow the parameters in a lambda expression defined in a Python-like style (`def`). If the colon is not found, an error is raised.

6. **Matching Arrow**:
   ```cpp
   if (!match(TokenType::FAT_ARROW))
       match(TokenType::ARROW);
   ```
   The parser then looks for either a fat arrow (`->`) or a regular arrow (`=>`) to denote the end of the parameter list and the start of the lambda body. If neither is found, an error is raised.

7. **Skipping Newlines**:
   ```cpp
   skipNewlines();
   ```
   Any newline characters encountered between the arrow and the body of the lambda are skipped to ensure proper parsing.

8. **Parsing Block Body**:
   ```cpp
   auto body = parseBlock();
   ```
   The `parseBlock` function is invoked to parse the block of code that constitutes the body of the lambda expression. This block may contain multiple statements or expressions.

9. **Constructing LambdaExpr Object**:
   ```cpp
   LambdaExpr le;
   le.params = std::move(params);
   le.paramTypes = std::move(paramTypes);
   le.defaultArgs = std::move(defaultArgs);
   le.body = std::move(body);
   ```
   A `LambdaExpr` object is constructed and populated with the parsed parameters, their types, default arguments, and the body of the lambda expression. The use of `std::move` ensures efficient transfer of resources from temporary objects to the final `LambdaExpr`.

10. **Returning ASTNode**:
    ```cpp
    return std::make_unique<ASTNode>(std::move(le), ln);
    ```
    Finally, the function constructs an `ASTNode` containing the `LambdaExpr` object and the captured line number. A unique pointer to this `ASTNode` is returned, ensuring that the ownership of the ASTNode is transferred safely.

## Edge Cases

- **Missing Colon**: If a colon is missing after the parameters in a Python-like lambda definition, the parser raises an error indicating an unexpected token.
- **Incorrect Arrow Type**: If neither a fat arrow nor a regular arrow is found to separate the parameters and the body, the parser raises an error indicating a syntax error.
- **Empty Lambda Body**: If the lambda body is empty, the parser still processes the lambda expression correctly, assuming it contains only a single statement or expression.

## Interactions with Other Components

- **Lexical Analyzer**: The `current()` function relies on the lexical analyzer to provide the current token being processed. This interaction ensures that the parser has accurate information about the syntax and structure of the input code.
- **Syntax Rules**: The `parseParamList` and `parseBlock` functions rely on predefined syntax rules to accurately parse the parameters and body of the lambda expression. These rules define the valid structures and tokens that can appear in a lambda expression.
- **Error Handling**: Throughout the process, the parser interacts with the error handling component to report syntax errors or unexpected tokens. This ensures that the user receives clear feedback when there are issues with the lambda expression syntax.

By understanding how `parseLambda` works and interacts with other components, developers can better appreciate the complexity involved in parsing lambda expressions and the importance of maintaining accurate and efficient parsing logic.