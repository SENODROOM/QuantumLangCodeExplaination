# Interpreter.cpp

## Overview

`Interpreter.cpp` is an essential part of the Quantum Language compiler, tasked with interpreting and executing quantum programs. This file houses the core logic of the interpreter, which processes quantum instructions and manages the execution environment.

## Role in Compiler Pipeline

This component operates at the execution phase of the compiler pipeline. It takes parsed quantum programs as input and executes them according to the rules defined in the language specification. The output of the interpreter can be used to simulate quantum operations or generate executable code for real quantum hardware.

## Key Design Decisions

### Use of `std::istringstream` for Token Buffering

**WHY**: The choice to use `std::istringstream` for token buffering stems from the behavior of `std::cin >>`, which reads whitespace-delimited tokens. In contrast, `std::getline` reads an entire line. To ensure that subsequent `std::cin >>` operations can correctly parse multiple tokens on a single line, we buffer the remaining tokens using `std::istringstream`. This allows us to maintain the expected behavior of `std::cin >>` without disrupting the flow of input handling.

## Major Classes/Functions Documentation

### Helper Functions

#### `toNum(const QuantumValue &v, const std::string &ctx)`

**Purpose**: Converts a `QuantumValue` to a numeric type (`double`). If the value is not a number, it throws a `TypeError`.

**Behavior**: 
- Checks if the `QuantumValue` is a number.
- Returns the numeric value if it is.
- Throws a `TypeError` with a context message if the value is not a number.

#### `toInt(const QuantumValue &v, const std::string &ctx)`

**Purpose**: Converts a `QuantumValue` to an integer type (`long long`). Utilizes `toNum` internally to perform the conversion.

**Behavior**: 
- Calls `toNum` to convert the `QuantumValue` to a `double`.
- Casts the result to a `long long` and returns it.
- Throws a `TypeError` with a context message if the value cannot be converted to a number.

### Format Engine

#### `applyFormat(const std::string &fmt, const std::vector<QuantumValue> &args, size_t argStart = 1)`

**Purpose**: Applies formatted strings to arguments, supporting various format specifiers such as `%d`, `%f`, `%s`, etc.

**Behavior**: 
- Iterates through the format string character by character.
- When encountering a format specifier, it extracts the corresponding argument from the `args` vector.
- Applies the specified formatting to the argument and appends the result to the output string.
- Handles different flags like `-` (left-align), `+` (force sign), `0` (zero-pad), etc., and applies them accordingly.
- Supports width and precision specifications for numerical values and strings.

**Tradeoffs/Limitations**:
- Limited support for complex format specifiers and flags beyond those listed.
- No support for locale-specific formatting.
- Potential performance overhead due to repeated parsing and formatting operations.

## Conclusion

`Interpreter.cpp` plays a pivotal role in the Quantum Language compiler by interpreting and executing quantum programs. Its design decisions, particularly the use of `std::istringstream` for token buffering, address specific challenges in handling user input. By documenting each major function and highlighting potential tradeoffs, this README provides a comprehensive understanding of the file's functionality and limitations.