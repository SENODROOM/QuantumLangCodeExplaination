# ParserLiterals Component

The `ParserLiterals` component is a crucial part of the Quantum Language (QL) compiler, responsible for parsing and interpreting different literal types as defined in the language's syntax. These include array literals, dictionary literals, lambda expressions, arrow functions, argument lists, parameter lists, C type keywords, and C-type variable declarations.

## Overview

### Functionality
- **parseArrayLiteral**: Parses array literals from the source code.
- **parseDictLiteral**: Parses dictionary literals from the source code.
- **parseLambda**: Parses lambda expressions from the source code.
- **parseArrowFunction**: Parses arrow functions from the source code.
- **parseArgList**: Parses argument lists from function calls or definitions.
- **parseParamList**: Parses parameter lists from function definitions.
- **isCTypeKeyword**: Determines if a given token is a C type keyword.
- **parseCTypeVarDecl**: Parses C-type variable declarations from the source code.

### File Structure
- Each function or feature is encapsulated in its own file within this directory.
- The files are named according to their functionality, making it easy to locate specific implementations.

### Flow
1. **Tokenization**: Before parsing literals, tokens are generated from the source code using a tokenizer.
2. **Parsing**:
   - **parseArrayLiteral**: Reads tokens until it encounters the closing bracket `[ ]`.
   - **parseDictLiteral**: Reads tokens until it encounters the closing brace `{ }`.
   - **parseLambda**: Identifies the lambda keyword and then parses the parameters and body.
   - **parseArrowFunction**: Recognizes the arrow function syntax and processes accordingly.
   - **parseArgList**: Parses arguments separated by commas.
   - **parseParamList**: Parses parameters separated by commas.
3. **Validation**: After parsing, the literals are validated against the language's rules.
4. **Interpretation**: Validated literals are interpreted into a structured format suitable for further compilation steps.

### Usage
To use the `ParserLiterals` component, simply include the appropriate header file and call the relevant function with the current token stream:

```cpp
#include "ParserLiterals.h"

// Example usage
if (isCTypeKeyword(currentToken)) {
    parseCTypeVarDecl();
} else if (currentToken == "[") {
    parseArrayLiteral();
}
```

## Contributing
Contributions to the `ParserLiterals` component are welcome! Please ensure your changes are well-documented and tested before submitting a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.