# `parseLambda`

## Purpose

The `parseLambda` function in the Quantum Language compiler is designed to parse lambda expressions, supporting both anonymous and named forms. This function captures essential details such as parameters, their types, default arguments, and the body of the lambda expression.

## Functionality

1. **Line Number Capture**: The function starts by capturing the current line number (`ln`) where the lambda expression begins using `current().line`.

2. **Default Arguments Initialization**: It initializes an empty vector `defaultArgs` to store any default arguments associated with the lambda's parameters.

3. **Parameter Types Initialization**: Another empty vector `paramTypes` is initialized to hold the data types of each parameter in the lambda expression.

4. **Parsing Parameter List**: The function calls `parseParamList` to extract the list of parameters. This method takes three pointers:
   - A pointer to a vector that will store the parsed AST nodes representing the parameters.
   - A pointer to a vector that will store the default values for these parameters.
   - A pointer to a vector that will store the types of these parameters.

5. **Colon Matching**: After parsing the parameter list, the function matches the colon (`:`) token, which is used in Python-style lambda definitions.

6. **Arrow Token Matching**: Depending on the syntax being used (JavaScript or Quantum), the function attempts to match either the fat arrow (`=>`) or the thin arrow (`->`). If neither is found, it defaults to matching the thin arrow.

7. **Skipping Newlines**: To ensure proper parsing, the function skips any newline characters following the arrow token using `skipNewlines()`.

8. **Parsing Body Block**: The function then parses the block of code that constitutes the body of the lambda expression using `parseBlock()`. This block can contain multiple statements.

9. **Creating Lambda Expression Object**: Once all necessary parts are parsed, a `LambdaExpr` object (`le`) is created. This object contains:
   - A vector of AST nodes representing the parameters.
   - A vector of strings representing the types of the parameters.
   - A vector of AST nodes representing the default arguments.
   - An AST node representing the body of the lambda expression.

10. **Returning Unique Pointer**: Finally, the function returns a unique pointer to an `ASTNode` containing the `LambdaExpr` object and the captured line number (`ln`).

## Parameters/Return Value

- **Parameters**:
  - None explicitly passed, but relies on global state managed by the parser.
  
- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` containing the parsed `LambdaExpr` and its starting line number.

## Edge Cases

- **Syntax Variations**: Handles different syntax variations like JavaScript’s `=>` and Quantum’s `->`.
- **Missing Elements**: Tolerates missing elements gracefully, such as default arguments or type declarations.
- **Empty Body**: Allows for an empty body, though this might be considered invalid depending on the context in which the lambda is used.

## Interactions with Other Components

- **Lexical Analyzer**: Relies on the lexical analyzer to tokenize the input source code correctly.
- **Parse Param List**: Invokes `parseParamList` to handle parameter extraction, which may involve interaction with other parsing functions.
- **Parse Block**: Uses `parseBlock` to parse the body of the lambda, potentially involving recursive parsing of nested blocks or statements.
- **Error Handling**: Integrates with error handling mechanisms to report syntax errors appropriately during parsing.

This comprehensive approach ensures that the `parseLambda` function accurately captures the nuances of lambda expressions across various syntax styles, providing robust support for the Quantum Language compiler.