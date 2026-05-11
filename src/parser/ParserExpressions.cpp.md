# ParserExpressions.cpp

## Overview

`ParserExpressions.cpp` is a crucial module within the Quantum Language Compiler, dedicated to parsing various expression types such as assignments, ternary operators, and tuple unpacking. This component plays a pivotal role in constructing accurate abstract syntax trees (ASTs), ensuring that the parsed code reflects its intended structure and semantics.

### Role in Compiler Pipeline

The parser is responsible for converting the input source code into a structured representation known as an AST. The `ParserExpressions.cpp` specifically focuses on parsing expressions, which are fundamental building blocks of any programming language. It interacts closely with other components of the compiler, such as the lexer, to tokenize the input and then constructs the AST based on those tokens.

### Key Design Decisions and Why

1. **Handling Ternary Operators**: The parser distinguishes between Python-style (`expr IF condition ELSE other_expr`) and JavaScript/Python-like (`condition ? thenExpr : elseExpr`) ternary operators. This decision allows the compiler to support multiple styles of conditional expressions, catering to different programming paradigms and user preferences.

2. **Tuple Unpacking Assignment**: To accurately parse tuple unpacking assignments (`a, b, c = expr`), the parser employs a non-consuming lookahead mechanism. This ensures that the parser does not prematurely consume tokens that could belong to function arguments or other contexts, thereby preventing false positives.

3. **Trade-offs**: One potential trade-off in handling ternary operators is the complexity introduced by distinguishing between different syntaxes. However, this approach enhances the flexibility and usability of the compiler, allowing it to support a wide range of programming languages and styles.

### Major Classes/Functions Overview

#### Parser Class

- **Functionality**: The `Parser` class encapsulates the logic for parsing the entire source code into an AST.
- **Key Functions**:
  - `parseAssignment()`: Parses assignment statements, including ternary expressions and tuple unpacking.
  - `parseOr()`, `parseExpr()`, etc.: Helper functions for parsing different types of expressions and statements.

#### ASTNode Class

- **Functionality**: Represents a node in the abstract syntax tree (AST). Each node corresponds to a part of the parsed code.
- **Key Members**:
  - `type`: Indicates the type of the node (e.g., Identifier, Assignment, TernaryExpression).
  - `children`: Stores pointers to child nodes, representing sub-expressions or parts of the statement.

### Tradeoffs

While the parser provides robust support for diverse expression types, it introduces some complexity due to the need to handle different syntaxes. For instance, distinguishing between Python-style and JavaScript-like ternary operators requires additional lookahead logic. This complexity must be balanced against the benefits of supporting a wide range of programming languages and styles, enhancing the overall usability and flexibility of the compiler. Additionally, the non-consuming lookahead mechanism used for tuple unpacking adds overhead but ensures precise parsing without unintended side effects.