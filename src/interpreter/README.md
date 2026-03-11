# Interpreter.cpp Documentation

## Overview

`Interpreter.cpp` is the core execution engine of the Quantum Language. This file implements the tree-walk interpreter that evaluates Abstract Syntax Tree (AST) nodes and executes Quantum scripts. It contains the main logic for statement execution, expression evaluation, and built-in function implementations.

## Architecture

The interpreter follows a tree-walk design pattern, traversing the AST recursively and evaluating each node according to its semantic meaning. The implementation is split into several key components:

### Core Components

1. **Environment Management**: Handles variable scoping and symbol tables
2. **Statement Execution**: Executes different types of statements (declarations, control flow, etc.)
3. **Expression Evaluation**: Evaluates expressions and computes values
4. **Built-in Functions**: Implements native functions and methods
5. **Type System**: Manages the dynamic type system and value operations

## Key Features

### 1. Format Engine
The interpreter includes a comprehensive printf-style format engine (`applyFormat` function) that supports:
- **Format Specifiers**: `%d`, `%i`, `%u`, `%f`, `%e`, `%g`, `%s`, `%c`, `%x`, `%X`, `%o`, `%b`, `%%`
- **Flags**: `-` (left-align), `+` (force sign), `0` (zero-pad), space, `#` (alternate form)
- **Width and Precision**: Customizable field width and decimal precision
- **Type Safety**: Automatic type conversion and validation

### 2. Input Buffering
Implements a smart input buffering system (`s_cinBuffer`) to handle C++-style token input (`cin >>`) properly, ensuring correct behavior with whitespace-delimited tokens.

### 3. Type Conversion Utilities
- `toNum()`: Safely converts QuantumValue to numeric types
- `toInt()`: Converts to integer with proper error handling
- Comprehensive type checking and error reporting

## Method Categories

### Statement Executors
- `execVarDecl()`: Variable declaration and initialization
- `execFunctionDecl()`: Function definition and registration
- `execClassDecl()`: Class definition and method setup
- `execIf()`: Conditional statement execution
- `execWhile()`: While loop execution with infinite loop protection
- `execFor()`: For loop execution
- `execReturn()`: Function return handling
- `execPrint()`: Print statement execution
- `execInput()`: User input handling
- `execImport()`: Module import functionality
- `execExprStmt()`: Expression statement evaluation

### Expression Evaluators
- `evalBinary()`: Binary operations (+, -, *, /, %, ==, !=, <, >, <=, >=, &&, ||)
- `evalUnary()`: Unary operations (!, -, +, ++, --)
- `evalAssign()`: Assignment operations (=, +=, -=, *=, /=, %=)
- `evalCall()`: Function and method calls
- `evalIndex()`: Array/string indexing and slicing
- `evalMember()`: Member access (dot notation)
- `evalArray()`: Array literal creation
- `evalDict()`: Dictionary/object literal creation
- `evalLambda()`: Lambda/arrow function creation
- `evalListComp()`: List comprehension evaluation
- `evalIdentifier()`: Variable resolution

### C++ Pointer Operations
- `evalAddressOf()`: Address-of operator (`&`)
- `evalDeref()`: Dereference operator (`*`)
- `evalArrow()`: Arrow operator (`->`)
- `evalNewExpr()`: Memory allocation (`new`)

### Method Dispatch System
- `callMethod()`: Generic method invocation
- `callArrayMethod()`: Array-specific methods (push, pop, sort, etc.)
- `callStringMethod()`: String manipulation methods
- `callDictMethod()`: Dictionary operations

## Built-in Function Categories

### Mathematical Functions
- Basic arithmetic: `abs()`, `sqrt()`, `pow()`, `exp()`, `log()`
- Trigonometric: `sin()`, `cos()`, `tan()`, `asin()`, `acos()`, `atan()`
- Hyperbolic: `sinh()`, `cosh()`, `tanh()`
- Constants: `PI`, `E`

### String Operations
- Case conversion: `upper()`, `lower()`, `capitalize()`
- Searching: `find()`, `index()`, `contains()`, `starts_with()`, `ends_with()`
- Manipulation: `replace()`, `split()`, `join()`, `slice()`, `repeat()`
- Formatting: `trim()`, `pad()`, `center()`
- JavaScript compatibility: `toUpperCase()`, `toLowerCase()`, `includes()`, etc.

### Array Operations
- Stack operations: `push()`, `pop()`, `peek()`
- Queue operations: `enqueue()`, `dequeue()`
- Sorting and searching: `sort()`, `reverse()`, `binary_search()`
- Manipulation: `slice()`, `concat()`, `unique()`

### I/O Operations
- File I/O: `read_file()`, `write_file()`, `append_file()`
- Console I/O: `print()`, `input()`, `printf()`
- Formatting: `format()`, `sprintf()`

### Cybersecurity Functions
- Encryption: `xor_bytes()`, `rot13()`, `caesar_cipher()`
- Hashing: `hash_string()`, `md5()`, `sha256()`
- Random generation: `random()`, `randint()`, `generate_random_bytes()`

### System Operations
- Time: `time()`, `sleep()`, `clock()`
- Process: `system()`, `exit()`
- Memory: `malloc()`, `free()`, `sizeof()`

## Performance Features

### Infinite Loop Protection
The interpreter includes a step counter (`stepCount_`) with a maximum step limit (`MAX_STEPS = 2,000,000`) to prevent infinite loops from hanging the system.

### Memory Management
- Smart pointers for automatic memory management
- Reference counting for shared data structures
- Efficient string operations with move semantics

### Error Handling
- Comprehensive type checking with descriptive error messages
- Runtime error detection and reporting
- Stack trace information for debugging

## Constants and Definitions

### Mathematical Constants
```cpp
#define M_PI 3.14159265358979323846
#define M_E 2.71828182845904523536
```

### Performance Limits
```cpp
static constexpr long long MAX_STEPS = 2'000'000;
```

## Usage Examples

### Function Registration
Built-in functions are automatically registered in the `registerNatives()` method, making them available in the global environment.

### Method Calls
The interpreter supports both native method calls and user-defined method calls through a unified dispatch system.

### Type Coercion
Automatic type conversion is performed for arithmetic operations, with proper error handling for invalid conversions.

## Dependencies

### Standard Library Includes
- `<iostream>`: Console I/O
- `<cmath>`: Mathematical functions
- `<algorithm>`: Standard algorithms
- `<unordered_set>`: Hash sets
- `<sstream>`: String streams
- `<fstream>`: File operations
- `<chrono>`: Time operations
- `<random>`: Random number generation
- `<functional>`: Function objects
- `<thread>`: Threading support
- `<regex>`: Regular expressions

### Internal Headers
- `"../include/Interpreter.h"`: Main interpreter interface
- `"../include/Error.h"`: Error handling definitions
- `"../include/Value.h"`: Value type system
- `"../include/AST.h"`: Abstract Syntax Tree definitions

## Design Patterns

### Visitor Pattern
The interpreter uses the visitor pattern to traverse and evaluate different AST node types.

### Environment Chain
Variable resolution follows a chain of environments, supporting proper lexical scoping.

### Method Dispatch
Dynamic method dispatch enables polymorphic behavior for built-in types.

## Thread Safety

The current implementation is single-threaded. The interpreter maintains a global state and environment chain that is not thread-safe for concurrent execution.

## Future Enhancements

Potential areas for improvement:
- Multi-threading support
- Just-in-time compilation
- Optimized bytecode generation
- Enhanced debugging capabilities
- Profiling and performance analysis tools
