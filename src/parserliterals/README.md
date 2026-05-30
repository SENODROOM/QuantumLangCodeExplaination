# ParserLiterals Component

The `ParserLiterals` component is a crucial module within the Quantum Language (QL) compiler, responsible for parsing and interpreting different literal types as defined by the language's syntax rules. This component ensures that the compiler can correctly understand and process literals such as arrays, dictionaries, lambdas, arrow functions, argument lists, parameter lists, C type keywords, and C-type variable declarations.

## Functionality Overview

- **parseArrayLiteral**: Parses an array literal from the source code.
- **parseDictLiteral**: Parses a dictionary literal from the source code.
- **parseLambda**: Parses a lambda expression from the source code.
- **parseArrowFunction**: Parses an arrow function from the source code.
- **parseArgList**: Parses an argument list from the source code.
- **parseParamList**: Parses a parameter list from the source code.
- **isCTypeKeyword**: Determines whether a given token is a C type keyword.
- **parseCTypeVarDecl**: Parses a C-type variable declaration from the source code.

## File Structure

Each function or utility in the `ParserLiterals` component is implemented in its own `.cpp` file and corresponding `.h` header file within this directory:

- `arrayliteral.cpp`, `arrayliteral.h`: Handles the parsing of array literals.
- `dictliteral.cpp`, `dictliteral.h`: Manages the parsing of dictionary literals.
- `lambda.cpp`, `lambda.h`: Processes lambda expressions.
- `arrowfunction.cpp`, `arrowfunction.h`: Supports arrow function parsing.
- `arglist.cpp`, `arglist.h`: Deals with argument list parsing.
- `paramlist.cpp`, `paramlist.h`: Manages parameter list parsing.
- `ctypekeywords.cpp`, `ctypekeywords.h`: Contains utilities to identify C type keywords.
- `ctypevardecl.cpp`, `ctypevardecl.h`: Handles the parsing of C-type variable declarations.

## Overall Flow

1. **Initialization**: The parser initializes by setting up necessary data structures and loading the source code.
2. **Tokenization**: Tokens are extracted from the source code using a tokenizer.
3. **Parsing Literals**:
   - For array literals, the `parseArrayLiteral` function is invoked.
   - For dictionary literals, the `parseDictLiteral` function processes them.
   - Lambda and arrow functions are parsed using `parseLambda` and `parseArrowFunction`, respectively.
   - Argument and parameter lists are handled by `parseArgList` and `parseParamList`.
4. **Type Checking**: After parsing, the `ParserLiterals` component checks the types of literals against the expected types in the context of the program.
5. **Error Handling**: If any errors occur during parsing, they are reported through error messages.
6. **Compilation**: Once all literals are successfully parsed and checked, the compilation proceeds to the next phase.

## Usage Example

```cpp
#include "parserliterals/arrayliteral.h"
#include "parserliterals/dictliteral.h"

int main() {
    // Initialize the parser with source code
    Parser parser("example.q");

    // Parse an array literal
    ArrayLiteral array = parser.parseArrayLiteral();

    // Parse a dictionary literal
    DictLiteral dict = parser.parseDictLiteral();

    return 0;
}
```

In this example, the `ParserLiterals` component is used to parse both array and dictionary literals from a hypothetical source code file named `example.q`.

## Contributing

Contributions to the `ParserLiterals` component are welcome! Please ensure your changes adhere to the project's coding standards and include appropriate unit tests.

---

This README provides a comprehensive overview of the `ParserLiterals` component, detailing its functionality, file structure, and overall flow within the Quantum Language compiler.