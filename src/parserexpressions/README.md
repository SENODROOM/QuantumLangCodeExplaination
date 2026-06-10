# ParserExpressions

The `ParserExpressions` component is a crucial module within the Quantum Language compiler, dedicated to parsing and interpreting different types of expressions encountered in quantum programs. This guarantees that quantum code conforms to appropriate syntax and semantics throughout its execution.

## Overview

The `ParserExpressions` module encompasses several functions designed to manage distinct expression types:

1. **parseAddSub**: Handles addition and subtraction operations.
2. **parseMulDiv**: Manages multiplication and division operations.
3. **parsePower**: Processes power operations.
4. **parseUnary**: Interprets unary operations.

These functions work together to ensure that complex expressions within quantum programs are accurately parsed and executed. The overall flow involves reading an expression from the input source, identifying its type, and then invoking the corresponding function to perform the operation. The results are then used to construct the quantum circuit or state representation as required by the compiler.

## Files and Functions

- **parseAddSub.cpp** and **parseAddSub.h**: Contains the implementation and declaration for parsing addition and subtraction expressions.
- **parseMulDiv.cpp** and **parseMulDiv.h**: Includes the implementation and declaration for handling multiplication and division expressions.
- **parsePower.cpp** and **parsePower.h**: Houses the implementation and declaration for processing power expressions.
- **parseUnary.cpp** and **parseUnary.h**: Features the implementation and declaration for interpreting unary expressions.

Each file is structured to provide clear separation between the interface and the implementation, facilitating maintenance and scalability of the module.

## Usage

To utilize the `ParserExpressions` component, include the necessary header files and call the appropriate function based on the type of expression you need to parse. For example:

```cpp
#include "parseAddSub.h"
#include "parseMulDiv.h"

int main() {
    // Example usage of parseAddSub
    Expression result = parseAddSub("5 + 3");
    
    // Example usage of parseMulDiv
    Expression result2 = parseMulDiv("4 * 6 / 2");
    
    return 0;
}
```

By integrating these functions into your quantum program, you can ensure that expressions are correctly interpreted and processed, leading to accurate compilation and execution of quantum circuits.

For more detailed information on each function and their parameters, refer to the individual header files (`*.h`).