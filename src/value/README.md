# Value Component of the Quantum Language Compiler

The Value component is an essential module in the Quantum Language (QL) compiler, responsible for handling and managing different types of values within the quantum computing environment. This component ensures efficient conversion, access, and manipulation of these values, facilitating their seamless integration across the compiler's architecture.

## Overview

The Value component provides a comprehensive set of functionalities to work with various data types encountered during the compilation process. It includes methods for checking truthiness, converting values to strings, retrieving type names, defining variables, accessing and modifying cell and field values, determining if a value is native, and converting native values back to QL values.

### Key Features

- **Value Conversion**: Efficiently converts between different data types used in quantum programming.
- **Variable Management**: Defines and manages variables, including both local and reference types.
- **Cell and Field Access**: Provides methods to get and set values at specific cells or fields.
- **Truthiness Check**: Determines whether a given value is considered "truthy" in the context of quantum programming.
- **Type Information**: Retrieves the type name of a value, aiding in type-checking and error handling.
- **Native Support**: Facilitates interaction with native quantum hardware and software libraries.

### Directory Structure

```
value/
├── include/
│   ├── ql/value.hpp
│   └── ...
├── src/
│   ├── ql/value.cpp
│   └── ...
├── tests/
│   ├── ql/value_test.cpp
│   └── ...
└── README.md
```

#### `include/ql/value.hpp`

This header file contains declarations for all public classes, structs, and functions related to the Value component. It serves as the primary interface for developers working with quantum values.

#### `src/ql/value.cpp`

The source file implements the functionality declared in `value.hpp`. It includes detailed logic for value conversions, variable management, and other operations.

#### `tests/ql/value_test.cpp`

Unit tests for the Value component, ensuring that all functions behave as expected under various conditions.

## Usage

To use the Value component in your quantum language projects, follow these steps:

1. **Include the Header File**:
    ```cpp
    #include <ql/value.hpp>
    ```

2. **Define Variables**:
    ```cpp
    ql::Value var = ql::Value::define("myVar", ql::ValueType::INTEGER);
    ```

3. **Set and Get Values**:
    ```cpp
    var.set(42);
    int value = var.get<int>();
    ```

4. **Check Truthiness**:
    ```cpp
    bool isTrue = var.isTruthy();
    ```

5. **Convert to String**:
    ```cpp
    std::string strValue = var.toString();
    ```

6. **Access Type Name**:
    ```cpp
    std::string typeName = var.typeName();
    ```

7. **Work with Cells and Fields**:
    ```cpp
    ql::Value cellValue = var.getCell("cell1");
    var.setField("field1", ql::Value::define("fieldValue", ql::ValueType::BOOLEAN));
    ```

8. **Interact with Native Values**:
    ```cpp
    ql::Value nativeVar = ql::Value::asNative(nativeQuantumObject);
    ```

## Contributing

Contributions to the Value component are welcome! If you find any issues or have suggestions for improvements, please open an issue on the [GitHub repository](https://github.com/your-repo/ql-value).

For more information on contributing, see our [CONTRIBUTING.md](https://github.com/your-repo/ql-value/blob/master/CONTRIBUTING.md) file.

## License

The Value component is licensed under the Apache License 2.0. See the [LICENSE](https://github.com/your-repo/ql-value/blob/master/LICENSE) file for details.

By using the Value component, you agree to be bound by the terms of this license.