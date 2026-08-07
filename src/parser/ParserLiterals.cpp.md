# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is an integral part of the Quantum Language compiler's parsing phase. Its primary function is to interpret and convert literal values such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`) into their corresponding Abstract Syntax Tree (AST) nodes. This conversion ensures that these literals are correctly represented within the AST, facilitating subsequent compilation stages.

## Key Design Decisions and Why

### Literal Handling
The parser handles different types of literals with specific functions like `parseNumber()`, `parseString()`, etc. Each function checks the type of the current token and processes it accordingly. For instance, numbers are parsed based on whether they are hexadecimal or decimal, and strings are handled directly.

**Why:** This approach allows for clear separation of concerns and makes the code easier to maintain and extend. By handling each literal type individually, we ensure that the parser can accurately interpret complex expressions involving multiple literals.

### Memory Allocation Parsing
Memory allocation literals (`new`) are parsed to create instances of objects. The parser expects a class name followed by parentheses containing arguments. If the syntax matches, it constructs an `ASTNode` representing the memory allocation.

**Why:** Properly parsing memory allocation literals is crucial for creating object instances during compilation. This design decision ensures that the parser can handle dynamic memory allocation correctly, which is a common feature in many programming languages.

### Tradeoffs
One potential tradeoff in this implementation is the complexity introduced by handling different literal types and memory allocation syntax. While this adds robustness to the parser, it also increases the overall codebase size and maintenance effort. However, the benefits of accurate literal interpretation and dynamic memory allocation outweigh this tradeoff for most practical use cases.

## Major Classes/Functions Overview

### Parser Class
- **Functionality:** Manages the parsing process, including token consumption and error handling.
- **Key Methods:** `parsePrimary()`, `parseAssignment()`, `expect()`.

### ASTNode Class
- **Functionality:** Represents a node in the Abstract Syntax Tree.
- **Types:** NumberLiteral, StringLiteral, BoolLiteral, NilLiteral, Identifier, LambdaExpr, BlockStmt, ReturnStmt.

### parsePrimary Function
- **Role:** Parses primary literals (numbers, strings, booleans, `nil`, `this`, `new`).
- **Implementation Details:** Uses conditional statements to check the type of the current token and calls appropriate helper functions to parse each literal type.

### parseAssignment Function
- **Role:** Parses assignment expressions, which may include literal values.
- **Implementation Details:** Handles the parsing of assignment operators and invokes `parsePrimary()` to parse the right-hand side of the assignment.

### expect Function
- **Role:** Ensures that the next token matches the expected type, throwing an error if not.
- **Implementation Details:** Takes an expected token type and a message string. Consumes the next token if it matches the expected type; otherwise, throws an error with the provided message.

## Conclusion

`ParserLiterals.cpp` is a vital component of the Quantum Language compiler, responsible for interpreting and converting literal values into AST nodes. Through careful design and implementation, it ensures accurate representation of literals and supports dynamic memory allocation, enhancing the compiler's functionality. While there are some complexities involved, the benefits of a robust parsing system make this design worthwhile.