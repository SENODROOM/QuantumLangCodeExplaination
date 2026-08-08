# QpmJson.cpp

## Role in Compiler Pipeline

`QpmJson.cpp` is a crucial component of the Quantum Programming Machine (QPM) compiler. It is responsible for parsing JSON documents into `JsonValue` objects, which can then be used throughout the compiler to represent and manipulate JSON data structures. This module plays a vital role in ensuring that the compiler can handle configuration files, metadata, and other JSON-based inputs effectively.

## Key Design Decisions and WHY

### Error Handling with Line and Column Information

One of the key design decisions in `QpmJson.cpp` is the implementation of detailed error handling. The `fail` function throws a `std::runtime_error` with information about the line and column where the error occurred. This decision is made to provide clear and actionable feedback during the compilation process, especially when dealing with complex configurations or large codebases.

### Use of Forward Declarations for Internal Classes

The internal `Parser` class is forward-declared within an anonymous namespace. This approach helps to keep the header file (`QpmJson.h`) clean and focused on the public API, while keeping the implementation details hidden from users of the library. By using forward declarations, we avoid unnecessary inclusion of implementation headers, which can lead to cleaner build systems and reduced compile times.

### Efficient Parsing of Different JSON Types

The `parseValue`, `parseObject`, `parseArray`, `parseString`, and `parseNumber` functions are designed to efficiently parse different types of JSON values. Each function handles its specific type, ensuring that the parser remains fast and responsive even when processing large JSON documents. This decision is critical for maintaining performance in the compiler's various stages, particularly those that involve extensive JSON manipulation.

## Major Classes/Functions Overview

### Parser Class

- **Role**: The primary class responsible for parsing JSON documents.
- **Methods**:
  - `explicit Parser(const std::string &text)`: Constructor initializes the parser with a JSON string.
  - `JsonValue parseDocument()`: Parses the entire JSON document and returns the root `JsonValue`.
  - `[[noreturn]] void fail(const std::string &msg)`: Throws an exception with detailed line and column information.
  - `char peek()`: Returns the next character without advancing the position.
  - `char next()`: Advances the position and returns the current character.
  - `void skipWs()`: Skips any whitespace characters in the JSON string.
  - `bool consumeLiteral(const char *lit)`: Consumes a literal string if it matches the current position.
  - `JsonValue parseValue()`: Parses a single JSON value based on its type.
  - `JsonValue parseObject()`: Parses a JSON object enclosed in curly braces `{}`.
  - `JsonValue parseArray()`: Parses a JSON array enclosed in square brackets `[]`.
  - `std::string parseString()`: Parses a JSON string enclosed in double quotes `"..."`.
  - `JsonValue parseNumber()`: Parses a JSON number, which can be either integer or floating-point.

### JsonValue Class

- **Role**: Represents a generic JSON value, which can be an object, array, string, boolean, null, or number.
- **Static Methods**:
  - `static JsonValue makeObject()`: Creates a new empty JSON object.
  - `static JsonValue makeArray()`: Creates a new empty JSON array.
  - `static JsonValue makeString(const std::string &str)`: Creates a JSON string value.
  - `static JsonValue makeBoolean(bool b)`: Creates a JSON boolean value.
  - `static JsonValue makeNull()`: Creates a JSON null value.
  - `static JsonValue makeNumber(double num)`: Creates a JSON number value.

## Tradeoffs

### Complexity vs. Usability

By hiding the implementation details behind forward declarations, we reduce the complexity of the public interface. However, this might slightly complicate the maintenance of the library, as developers need to understand the forward declaration mechanism to work with internal classes.

### Performance vs. Memory Usage

Efficient parsing of different JSON types ensures good performance, but it might also increase memory usage due to the creation of multiple intermediate `JsonValue` objects. Balancing these factors requires careful consideration of the use cases and optimization strategies employed in the parser.

### Error Handling vs. Robustness

Detailed error handling provides robustness by giving clear feedback on issues encountered during parsing. However, it might add overhead to the parsing process, making it slower. Finding the right balance between thorough error checking and performance is essential for the overall usability of the compiler.

In conclusion, `QpmJson.cpp` is a well-designed and efficient part of the QPM compiler, providing robust JSON parsing capabilities with detailed error handling and a clean public API. Its role in the compiler pipeline is vital for handling various JSON-based inputs, and its design decisions reflect a thoughtful balance between performance, usability, and maintainability.