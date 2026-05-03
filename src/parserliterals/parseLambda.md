# `parseLambda`

## Purpose

The `parseLambda` function is designed to parse a lambda expression within the Quantum Language compiler. This function supports both anonymous and named forms of lambda expressions, handling the capture of parameters, their types, default arguments, and the body of the lambda.

## Parameters

- **None**: The `parseLambda` function does not take any explicit parameters. Instead, it relies on the global state managed by the parser, such as the current token being processed (`current()`), which provides information about the syntax elements encountered during parsing.

## Return Value

The function returns a unique pointer to an `ASTNode` representing the parsed lambda expression. The `ASTNode` contains a `LambdaExpr` object, which encapsulates:
- `params`: A vector of pointers to `ASTNode` objects representing the parameters of the lambda.
- `paramTypes`: A vector of strings indicating the types of each parameter.
- `defaultArgs`: A vector of pointers to `ASTNode` objects representing any default arguments provided for the parameters.
- `body`: A pointer to an `ASTNode` object representing the body of the lambda expression.

## Edge Cases

1. **Default Arguments**: If a parameter has a default argument, it should be correctly identified and captured. The function uses the `defaultArgs` vector to store these defaults.
2. **Parameter Types**: The function must accurately determine the type of each parameter. This is crucial for type checking and ensuring that the lambda adheres to the expected data types.
3. **Syntax Variations**: The function should handle different syntax variations for lambda expressions, such as the use of `->`, `=>`, and `:` to denote the end of the parameter list and the start of the body.

## Interactions with Other Components

- **Tokenizer**: The `parseLambda` function consumes tokens from the tokenizer. It relies on the tokenizer's ability to identify keywords, colons, arrows, and other syntax elements relevant to lambda expressions.
- **Parser State Management**: The function operates within the context of the parser's state, which includes managing the current line number (`ln`) and maintaining a stack of nodes being constructed.
- **Error Handling**: During parsing, the function may encounter errors related to incorrect syntax or missing elements. Error handling mechanisms within the parser will manage these situations appropriately.
- **AST Construction**: The function constructs an abstract syntax tree (AST) node for the lambda expression. This AST node is then used by subsequent stages of compilation to generate machine code or perform further analysis.

## Detailed Explanation

### Step-by-Step Parsing Process

1. **Line Number Capture**:
   ```cpp
   int ln = current().line;
   ```
   The function captures the current line number where the lambda expression begins. This is important for error reporting and debugging purposes.

2. **Parsing Parameter List**:
   ```cpp
   auto params = parseParamList(nullptr, &defaultArgs, &paramTypes);
   ```
   The `parseParamList` function is called to parse the list of parameters for the lambda. This function populates the `params`, `defaultArgs`, and `paramTypes` vectors based on the syntax encountered.

3. **Matching Syntax Elements**:
   ```cpp
   match(TokenType::COLON); // Python: def style
   if (!match(TokenType::FAT_ARROW))
       match(TokenType::ARROW); // JS => or Quantum ->
   ```
   The function ensures that the appropriate syntax element (either `:` or `->`) is present to mark the end of the parameter list and the beginning of the lambda body. This step is critical for distinguishing between different programming languages' syntax conventions.

4. **Skipping Newlines**:
   ```cpp
   skipNewlines();
   ```
   After matching the syntax element, the function skips any newlines that might follow. This helps maintain consistent parsing behavior regardless of formatting differences.

5. **Parsing Lambda Body**:
   ```cpp
   auto body = parseBlock();
   ```
   The `parseBlock` function is invoked to parse the body of the lambda expression. This block can contain multiple statements or expressions depending on the language's rules.

6. **Creating Lambda Expression Node**:
   ```cpp
   LambdaExpr le;
   le.params = std::move(params);
   le.paramTypes = std::move(paramTypes);
   le.defaultArgs = std::move(defaultArgs);
   le.body = std::move(body);
   return std::make_unique<ASTNode>(std::move(le), ln);
   ```
   Finally, the function creates a `LambdaExpr` object containing all the parsed information and constructs an `ASTNode` around it. The line number (`ln`) is also stored in the `ASTNode` to provide context for potential errors or issues during compilation.

By following this structured approach, the `parseLambda` function effectively parses complex lambda expressions while maintaining compatibility across different syntax styles. This ensures that the Quantum Language compiler can handle a wide range of input formats and produce accurate AST representations for further processing.