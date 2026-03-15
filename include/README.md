# QuantumLanguage Compiler - Token.h

## Overview

The `include/Token.h` header file is an essential part of the QuantumLanguage compiler, focusing on the representation and handling of tokens within the language. Tokens serve as the fundamental building blocks for parsing and interpreting the source code. This file defines the `TokenType` enum class and the `Token` struct, which encapsulate the type, value, and location information of each token.

## Key Design Decisions

### TokenType Enum Class

- **Purpose**: The `TokenType` enum class categorizes different types of tokens used in QuantumLanguage. Each category corresponds to a specific token type.
  
- **Why**: This design decision allows for clear and unambiguous identification of token types, facilitating the parsing process and ensuring that the correct actions are taken based on the token's nature.

- **Trade-offs**: While this approach provides a comprehensive list of token types, it may lead to increased complexity if new token types need to be added frequently. Additionally, maintaining this list requires careful consideration to avoid conflicts between existing categories.

### Token Struct

- **Purpose**: The `Token` struct represents a single token in the source code. It contains the token's type, value, and position (line and column numbers).
  
- **Why**: This design choice enables efficient storage and retrieval of token information during the parsing phase. By storing the line and column numbers, the compiler can provide precise error messages when syntax errors occur.

- **Trade-offs**: Storing the entire token value as a string might consume more memory than necessary, especially for large tokens. However, this approach simplifies token comparison and manipulation operations.

## Documentation

### TokenType Enum Class

```cpp
enum class TokenType
{
    // Literals
    NUMBER,          // Represents numeric literals
    STRING,          // Represents string literals
    TEMPLATE_STRING, // Backtick template literal segment (text before ${)
    BOOL_TRUE,       // Represents the boolean true value
    BOOL_FALSE,      // Represents the boolean false value
    NIL,             // Represents the nil value

    // Identifiers & Keywords
    IDENTIFIER,      // Represents variable names and identifiers
    LET,             // Keyword for variable declaration
    CONST,           // Keyword for constant declaration
    FN,              // Keyword for function definition
    DEF,             // Python: def
    FUNCTION,        // JavaScript: function
    CLASS,           // Class keyword
    EXTENDS,         // Extends / Inherits keyword
    NEW,             // New keyword
    THIS,            // This keyword (JavaScript alias for self)
    SUPER,           // Super keyword
    RETURN,          // Return keyword
    IF,              // If keyword
    ELSE,            // Else keyword
    ELIF,            // Elif keyword
    WHILE,           // While loop keyword
    FOR,             // For loop keyword
    IN,              // In keyword
    OF,              // JavaScript for...of keyword
    BREAK,           // Break statement keyword
    CONTINUE,        // Continue statement keyword
    RAISE,           // Raise exception keyword
    TRY,             // Try block keyword
    EXCEPT,          // Except block keyword
    FINALLY,         // Finally block keyword
    AS,              // As keyword
    PRINT,           // Print statement keyword
    INPUT,           // Input statement keyword
    COUT,            // C++ cout keyword
    CIN,             // C++ cin keyword
    FROM,            // From keyword
    IMPORT,          // Import statement keyword

    // C/C++ style type keywords
    TYPE_INT,        // Integer type
    TYPE_FLOAT,      // Floating-point type
    TYPE_DOUBLE,     // Double precision floating-point type
    TYPE_CHAR,       // Character type
    TYPE_STRING,     // String type
    TYPE_BOOL,       // Boolean type
    TYPE_VOID,       // Void type
    TYPE_LONG,       // Long integer type
    TYPE_SHORT,      // Short integer type
    TYPE_UNSIGNED,   // Unsigned integer type

    // Operators
    PLUS,            // Addition operator
    MINUS,           // Subtraction operator
    STAR,            // Multiplication operator
    SLASH,           // Division operator
    FLOOR_DIV,       // Integer division operator (Python)
    PERCENT,         // Modulus operator
    POWER,           // Exponentiation operator
    EQ,              // Equality operator
    NEQ,             // Inequality operator
    STRICT_EQ,       // Strict equality operator
    STRICT_NEQ,      // Strict inequality operator
    NULL_COALESCE,   // Null coalescing operator (??
    LT,              // Less than operator
    GT,              // Greater than operator
    LTE,             // Less than or equal to operator
    GTE,             // Greater than or equal to operator
    AND,             // Logical AND operator
    OR,              // Logical OR operator
    NOT,             // Logical NOT operator
    IS,              // Identity operator
    ASSIGN,          // Assignment operator
    PLUS_ASSIGN,     // Addition assignment operator
    MINUS_ASSIGN,    // Subtraction assignment operator
    STAR_ASSIGN,     // Multiplication assignment operator
    SLASH_ASSIGN,    // Division assignment operator
    AND_ASSIGN,      // Bitwise AND assignment operator
    OR_ASSIGN,       // Bitwise OR assignment operator
    XOR_ASSIGN,      // Bitwise XOR assignment operator
    MOD_ASSIGN,      // Modulus assignment operator
    FAT_ARROW,       // Fat arrow operator (=>)
    PLUS_PLUS,       // Increment operator
    MINUS_MINUS,     // Decrement operator
    BIT_AND,         // Bitwise AND operator
    BIT_OR,          // Bitwise OR operator
    BIT_XOR,