# parserliterals

The `parserliterals` component is an integral part of the Quantum Language (QL) compiler, designed to parse and interpret various literal types within the language's syntax. This includes array literals, dictionary literals, lambda expressions, arrow functions, argument lists, parameter lists, C type keywords, and C-type variable declarations.

## Directory Structure

- **parseArrayLiteral.cpp**: Contains the implementation for parsing array literals in the Quantum Language.
- **parseDictLiteral.cpp**: Implements the parsing logic for dictionary literals.
- **parseLambda.cpp**: Handles the parsing of lambda expressions.
- **parseArrowFunction.cpp**: Parses arrow function syntax.
- **parseArgList.cpp**: Manages the parsing of argument lists used in function calls or definitions.
- **parseParamList.cpp**: Responsible for parsing parameter lists in function definitions.
- **isCTypeKeyword.h**: Provides utility functions to check if a given token is a C type keyword.
- **parseCTypeVarDecl.cpp**: Implements the parsing of C-type variable declarations.

## Overall Flow

1. **Initialization**: The parser starts by initializing necessary data structures and variables required for parsing.
2. **Tokenization**: Tokens are generated from the input source code using a tokenizer.
3. **Parsing Literals**:
   - **Array Literals**: The `parseArrayLiteral` function processes tokens to construct an array literal.
   - **Dictionary Literals**: Similarly, the `parseDictLiteral` function constructs dictionary literals from tokens.
4. **Handling Functions**:
   - **Lambda Expressions**: The `parseLambda` function parses lambda expressions, which are anonymous functions defined inline.
   - **Arrow Functions**: Arrow functions are parsed by the `parseArrowFunction` function, providing a concise way to define functions.
5. **Parameter and Argument Lists**:
   - **Parameter Lists**: The `parseParamList` function handles the parsing of parameters in function definitions.
   - **Argument Lists**: The `parseArgList` function processes arguments when calling functions.
6. **C-Type Keywords and Declarations**:
   - **C Type Keywords**: The `isCTypeKeyword` function checks if a token represents a C type keyword.
   - **C-Type Variable Declarations**: The `parseCTypeVarDecl` function parses variable declarations that use C type keywords.

Each file in the `parserliterals` component plays a specific role in the parsing process, ensuring that all literal types are correctly interpreted according to the Quantum Language's grammar rules.

## Usage

To utilize the `parserliterals` component, include the appropriate header files in your project and call the relevant parsing functions as needed during the compilation process.

```cpp
#include "parserliterals/parseArrayLiteral.h"
#include "parserliterals/parseDictLiteral.h"
#include "parserliterals/parseLambda.h"
#include "parserliterals/parseArrowFunction.h"
#include "parserliterals/parseArgList.h"
#include "parserliterals/parseParamList.h"
#include "parserliterals/isCTypeKeyword.h"
#include "parserliterals/parseCTypeVarDecl.h"

// Example usage
void parseExample() {
    // Tokenize the input source code
    std::vector<Token> tokens = tokenize("..."); // Assume tokenize function exists

    // Parse array literal
    ArrayLiteral arrayLiteral = parseArrayLiteral(tokens);

    // Parse dictionary literal
    DictLiteral dictLiteral = parseDictLiteral(tokens);

    // Parse lambda expression
    LambdaExpression lambdaExpr = parseLambda(tokens);

    // Parse arrow function
    ArrowFunction arrowFunc = parseArrowFunction(tokens);

    // Parse argument list
    ArgList argList = parseArgList(tokens);

    // Parse parameter list
    ParamList paramList = parseParamList(tokens);

    // Check if token is a C type keyword
    bool isKeyword = isCTypeKeyword("int");

    // Parse C-type variable declaration
    CTypeVarDeclaration cTypeVarDecl = parseCTypeVarDecl(tokens);
}
```

By leveraging the functionalities provided by the `parserliterals` component, developers can ensure that their Quantum Language code is accurately parsed and compiled into executable quantum programs.