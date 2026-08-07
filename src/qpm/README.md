# Quantum Package Manager (QPM) JSON Library

The `src/qpm/QpmJson.h` file provides a minimal, self-contained JSON value type along with a parser and serializer tailored for use in the Quantum Package Manager (QPM). This library is designed to handle JSON data relevant to npm registry responses, `package.json`, and `qpm-lock.json`. One of its key features is that objects preserve insertion order, ensuring predictable behavior when serializing back to JSON.

## Role in Compiler Pipeline

This file plays a crucial role in the QPM compiler pipeline by providing a robust JSON handling mechanism. It allows QPM to parse and serialize JSON data efficiently, which is essential for managing packages and their dependencies.

## Key Design Decisions and Why

1. **Minimalism**: The library is kept minimal to reduce overhead and ensure simplicity in integration within the QPM compiler pipeline.
2. **Self-Contained**: All necessary components are included within the header file itself, making it easy to include and use without external dependencies.
3. **Preserving Insertion Order**: By using `std::vector` for both arrays and objects, the library ensures that elements maintain their original order, which is important for maintaining consistency across different serialization processes.
4. **Safe Access Methods**: The `get` method provides a way to safely access values in an object, returning a default value if the key does not exist or if the value is not of the expected type.

## Major Classes/Functions Overview

### `JsonValue`

The core class representing a JSON value. It can hold various types including null, boolean, number, string, array, and object. Each type has corresponding methods to check and convert the value.

#### Types Enum

- `Null`: Represents a JSON null value.
- `Bool`: Represents a JSON boolean value.
- `Number`: Represents a JSON numeric value (both integer and floating-point).
- `String`: Represents a JSON string value.
- `Array`: Represents a JSON array value.
- `Object`: Represents a JSON object value.

#### Constructors

- Default constructor initializes the value to null.
- Constructors for boolean, number, and string types initialize the value accordingly.

#### Static Methods

- `makeArray()`: Creates a new `JsonValue` initialized as an empty array.
- `makeObject()`: Creates a new `JsonValue` initialized as an empty object.

#### Methods

- `type()`: Returns the type of the JSON value.
- `isNull()`, `isBool()`, `isNumber()`, `isString()`, `isArray()`, `isObject()`: Check if the value is of the specified type.
- `asBool(bool def = false)`, `asNumber(double def = 0.0)`, `asString()`, `asString(const std::string &def)`: Safely convert the value to the specified type, returning a default value if the conversion is not possible.
- `array()`, `object()`: Return references to the internal array or object, allowing modification.
- `find(const std::string &key)`: Searches for a key in an object and returns a pointer to the corresponding `JsonValue` if found, otherwise returns `nullptr`.
- `get(const std::string &key, const JsonValue &def = JsonValue())`: Retrieves the value associated with a key in an object, returning a default value if the key is absent or the value is not of the expected type.

### Tradeoffs

- **Simplicity vs. Flexibility**: While the library is minimalistic, it sacrifices some flexibility compared to more comprehensive JSON libraries like RapidJSON or nlohmann/json. However, this tradeoff is justified by the need for a lightweight solution that integrates well with the QPM compiler pipeline.
- **Performance vs. Memory Usage**: The use of `std::vector` for arrays and objects ensures good performance but may lead to higher memory usage compared to other data structures. This tradeoff is acceptable given the typical size of JSON data handled by QPM.
- **Error Handling**: The library uses exceptions sparingly, focusing on providing clear error messages through its methods. This approach simplifies error handling but may not be suitable for all applications requiring strict control over error management.

Overall, the `QpmJson.h` file offers a compact yet effective solution for JSON processing in the QPM compiler pipeline, balancing simplicity, performance, and error handling needs.