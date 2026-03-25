# `parseLambda`

## Purpose

The `parseLambda` function is responsible for parsing a lambda expression in the Quantum Language compiler. It handles both anonymous and named forms of lambda expressions, focusing on capturing the parameters, their types, any default arguments, and the body of the lambda function.

## Functionality

1. **Initialization**:
   - The function starts by recording the current line number (`ln`) using `current().line`.

2. **Parsing Parameters**:
   - It calls `parseParamList` to extract the list of parameters, default arguments, and parameter types. This function takes three pointers as arguments: one for storing default arguments (`defaultArgs`), another for storing parameter types (`paramTypes`), and the third for storing the parsed parameters (`params`). If no type information is provided, `parseParamList` will handle that gracefully.

3. **Matching Colon**:
   - The function then matches a colon token (`TokenType::COLON`). This is typical in languages like Python where lambda functions are defined using the syntax `def lambda_name(parameters):`.

4. **Matching Arrow**:
   - Next, it attempts to match a fat arrow (`TokenType::FAT_ARROW`). If this token is not found, it falls back to matching a regular arrow (`TokenType::ARROW`). This flexibility allows the parser to accommodate different syntax styles used in various programming languages (e.g., JavaScript's `=>`, Quantum's `->`).

5. **Skipping Newlines**:
   - After successfully matching the arrow token, the function skips any newline characters using `skipNewlines()`. This ensures that the parser can correctly handle multi-line lambda bodies.

6. **Parsing Body**:
   - The function then parses the body of the lambda expression using `parseBlock()`. This block represents the code that will be executed when the lambda is invoked.

7. **Constructing Lambda Expression**:
   - Once all necessary parts of the lambda expression have been parsed, the function constructs a `LambdaExpr` object (`le`). This object contains:
     - `params`: A vector of AST nodes representing the lambda's parameters.
     - `paramTypes`: A vector of strings representing the types of each parameter.
     - `defaultArgs`: A vector of AST nodes representing any default arguments provided for the parameters.
     - `body`: An AST node representing the body of the lambda function.

8. **Returning AST Node**:
   - Finally, the function returns a unique pointer to an `ASTNode` containing the constructed `LambdaExpr` and the recorded line number (`ln`).

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the function signature, but it relies on the `parseParamList` function which takes three pointers (`nullptr`, `&defaultArgs`, `&paramTypes`).
  
- **Return Value**:
  - Returns a `unique_ptr<ASTNode>` containing the parsed lambda expression and its line number.

## Edge Cases

- **No Default Arguments**: If there are no default arguments, `defaultArgs` will remain empty.
- **Missing Parameter Types**: If parameter types are not specified, `paramTypes` will contain empty strings for those positions.
- **Single-Line vs Multi-Line Bodies**: The function correctly handles both single-line and multi-line lambda bodies by skipping newlines before parsing the body.

## Interactions with Other Components

- **Tokenizer**: The function uses tokens consumed by the tokenizer to determine the structure of the lambda expression.
- **Parse Param List**: It interacts with the `parseParamList` function to extract parameter details such as names, types, and default values.
- **Parse Block**: It collaborates with the `parseBlock` function to parse the body of the lambda expression, which could include multiple statements or expressions.
- **Error Handling**: While not shown in the snippet, the function likely integrates with error handling mechanisms to manage unexpected tokens or structures during parsing.

This comprehensive approach ensures that `parseLambda` can accurately parse a wide range of lambda expressions in the Quantum Language, accommodating different syntax styles and providing robust error management.