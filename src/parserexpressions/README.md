# ParserExpressions

The `ParserExpressions` component is an essential part of the Quantum Language compiler, tasked with parsing and interpreting expressions found within quantum programs. This module encompasses a diverse array of functions tailored to manage various operator and expression types, thereby guaranteeing accurate syntax and semantic understanding as quantum code executes.

## Overview

The `ParserExpressions` component serves as the core engine for evaluating quantum expressions. It processes input strings representing quantum operations and translates them into executable instructions. The component operates in conjunction with other modules such as `Lexer`, `SymbolTable`, and `Optimizer`.

### Flowchart

```
Input String -> Lexer -> Tokens -> ParserExpressions -> AST -> Optimizer -> Executable Code
```

1. **Lexer**: Converts the input string into tokens based on language grammar rules.
2. **Tokens**: A sequence of token objects representing individual elements of the input string.
3. **ParserExpressions**: Parses the tokens to construct an Abstract Syntax Tree (AST) that represents the structure of the quantum expression.
4. **AST**: A tree-like representation of the parsed quantum expression, facilitating further processing and optimization.
5. **Optimizer**: Refines the AST to enhance performance and efficiency.
6. **Executable Code**: Translated AST into optimized quantum code ready for execution.

## Functions/Files

- **parseAssignment**: Handles assignment expressions like `a = b`.
- **parseOr**: Processes logical OR operations (`||`).
- **parseAnd**: Manages logical AND operations (`&&`).
- **parseBitwise**: Interprets bitwise operations like `&`, `|`, `^`, `~`, `<<`, and `>>`.
- **parseEquality**: Evaluates equality checks using `==` and `!=`.
- **parseComparison**: Performs comparison operations such as `<`, `<=`, `>`, and `>=`.
- **parseShift**: Handles shift operations (`<<` and `>>`).
- **parseAddSub**: Processes addition and subtraction operations (`+` and `-`).
- **parseMulDiv**: Manages multiplication and division operations (`*` and `/`).
- **parsePower**: Interprets power operations (`**`).
- **parseUnary**: Handles unary operations like `!`, `-`, `+`, `~`, `++`, and `--`.

Each function is meticulously designed to parse specific types of expressions and ensure they adhere to the Quantum Language's syntax and semantics.

## Usage

To utilize the `ParserExpressions` component, include it in your project and call the appropriate function based on the type of expression you wish to parse:

```cpp
#include "ParserExpressions.h"

int main() {
    std::string input = "a = b + c";
    ASTNode* ast = parseAssignment(input);
    // Further processing...
    return 0;
}
```

By leveraging the `ParserExpressions` component, developers can effectively parse and interpret quantum expressions, enabling the creation of robust and efficient quantum programs.