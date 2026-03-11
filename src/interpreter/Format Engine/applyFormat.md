# applyFormat() Function Documentation

## Overview

The `applyFormat()` function is a comprehensive printf-style formatting engine that powers the Quantum Language's string formatting capabilities. It supports a wide range of format specifiers, flags, width, and precision options, making it compatible with C-style printf formatting while adding Quantum-specific extensions.

## Function Signature

```cpp
static std::string applyFormat(const std::string &fmt, const std::vector<QuantumValue> &args, size_t argStart = 1)
```

## Parameters

- **`fmt`** (`const std::string &`): The format string containing format specifiers
- **`args`** (`const std::vector<QuantumValue> &`): Vector of arguments to be formatted
- **`argStart`** (`size_t`, default: 1): Index in args vector to start from (for printf-style functions)

## Return Value

- **`std::string`**: The formatted string with all format specifiers replaced by corresponding argument values

## Supported Format Specifiers

### Standard C Specifiers
- **`%d`, `%i`**: Signed decimal integers
- **`%u`**: Unsigned decimal integers
- **`%f`, `%F`**: Floating-point decimal notation
- **`%e`, `%E`**: Scientific notation
- **`%g`, `%G`**: General format (chooses between `%f` and `%e`)
- **`%s`**: Strings
- **`%c`**: Characters
- **`%x`, `%X`**: Hexadecimal notation (lowercase/uppercase)
- **`%o`**: Octal notation
- **`%%`**: Literal percent sign

### Quantum Extensions
- **`%b`**: Binary notation (non-standard extension)
- **`%B`**: Boolean values ("true"/"false")
- **`%t`**: Type name of the argument

## Supported Flags

| Flag | Description | Example |
|------|-------------|---------|
| `-` | Left-align output | `%-10s` |
| `+` | Force sign for positive numbers | `%+d` |
| `space` | Space for positive numbers | `% d` |
| `0` | Zero-pad numeric values | `%08d` |
| `#` | Alternate form (prefix for hex/octal/binary) | `%#x`, `%#o`, `%#b` |

## Width and Precision

### Width
Specifies minimum field width:
- `%8d`: Minimum 8 characters wide
- `%-10s`: Minimum 10 characters, left-aligned

### Precision
Controls number precision:
- `%6.2f`: 2 decimal places, minimum 6 characters
- `%.5s`: Maximum 5 characters from string
- `%08.3f`: 3 decimal places, zero-padded to 8 characters

## Implementation Architecture

### Main Loop Structure
```cpp
while (i < fmt.size()) {
    if (fmt[i] != '%') {
        out += fmt[i++];  // Copy literal characters
        continue;
    }
    // Handle format specifier...
}
```

### Argument Management
```cpp
auto nextArg = [&]() -> QuantumValue {
    return argIdx < args.size() ? args[argIdx++] : QuantumValue();
};
```

### Parsing Pipeline
1. **Flag Collection**: Parse all flag characters
2. **Width Parsing**: Extract field width
3. **Precision Parsing**: Extract precision value
4. **Conversion**: Apply format specifier
5. **Padding**: Apply width/alignment

## Format Specifier Implementations

### Integer Specifiers (`%d`, `%i`)
```cpp
case 'd':
case 'i': {
    long long n = arg.isNumber() ? (long long)arg.asNumber() : 0LL;
    // Handle precision and flags
    std::snprintf(buf, sizeof(buf), "%lld", n);
    // Apply sign flags and padding
    out += pad(s, true);
    break;
}
```

### Floating-Point Specifiers (`%f`, `%e`, `%g`)
```cpp
case 'f':
case 'F': {
    double d = arg.isNumber() ? arg.asNumber() : 0.0;
    std::string spec = "%";
    if (flagPlus) spec += '+';
    if (prec >= 0) spec += "." + std::to_string(prec);
    spec += 'f';
    std::snprintf(buf, sizeof(buf), spec.c_str(), d);
    out += pad(std::string(buf), true);
    break;
}
```

### String Specifier (`%s`)
```cpp
case 's': {
    std::string s = arg.toString();
    if (prec >= 0 && (int)s.size() > prec)
        s = s.substr(0, prec); // Truncate to precision
    out += pad(s, false);
    break;
}
```

### Character Specifier (`%c`)
```cpp
case 'c': {
    char c = arg.isString() && !arg.asString().empty()
                 ? arg.asString()[0]
                 : (char)(arg.isNumber() ? (int)arg.asNumber() : 0);
    out += pad(std::string(1, c), false);
    break;
}
```

### Hexadecimal Specifiers (`%x`, `%X`)
```cpp
case 'x':
case 'X': {
    unsigned long long n = (unsigned long long)(long long)(arg.isNumber() ? arg.asNumber() : 0.0);
    std::string spec = "%";
    if (flagHash) spec += '#';
    if (prec >= 0) spec += "." + std::to_string(prec);
    spec += (conv == 'x') ? "llx" : "llX";
    std::snprintf(buf, sizeof(buf), spec.c_str(), n);
    // Add 0x/0X prefix if flagHash is set
    out += pad(s, true);
    break;
}
```

### Binary Specifier (`%b`) - Quantum Extension
```cpp
case 'b': {
    long long n = arg.isNumber() ? (long long)arg.asNumber() : 0LL;
    if (n == 0) {
        out += pad(flagHash ? "0b0" : "0", true);
    } else {
        std::string bits;
        unsigned long long u = (unsigned long long)n;
        while (u) {
            bits = (char)('0' + (u & 1)) + bits;
            u >>= 1;
        }
        if (flagHash) bits = "0b" + bits;
        out += pad(bits, true);
    }
    break;
}
```

### Boolean Specifier (`%B`) - Quantum Extension
```cpp
case 'B': {
    out += pad(arg.isTruthy() ? "true" : "false", false);
    break;
}
```

### Type Name Specifier (`%t`) - Quantum Extension
```cpp
case 't': {
    out += pad(arg.typeName(), false);
    break;
}
```

## Padding Logic

The padding function handles alignment and zero-padding:
```cpp
auto pad = [&](std::string s, bool numericSign = false) -> std::string {
    if (width > 0 && (int)s.size() < width) {
        int pad = width - (int)s.size();
        if (flagMinus)
            s += std::string(pad, ' ');                    // Left-align
        else if (flagZero && numericSign)
            s = std::string(pad, '0') + s;                  // Zero-pad numbers
        else
            s = std::string(pad, ' ') + s;                  // Right-align
    }
    return s;
};
```

## Usage Examples

### Basic Formatting
```cpp
std::vector<QuantumValue> args = {42, 3.14159, "Hello"};
std::string result = applyFormat("Number: %d, Pi: %.2f, Text: %s", args);
// Result: "Number: 42, Pi: 3.14, Text: Hello"
```

### Flags and Width
```cpp
std::vector<QuantumValue> args = {42, -42};
std::string result = applyFormat("Positive: %+5d, Negative: %-5d", args);
// Result: "Positive:   +42, Negative: -42  "
```

### Quantum Extensions
```cpp
std::vector<QuantumValue> args = {true, 42, "Hello"};
std::string result = applyFormat("Bool: %B, Binary: %#b, Type: %t", args);
// Result: "Bool: true, Binary: 0b101010, Type: number"
```

### Precision Control
```cpp
std::vector<QuantumValue> args = {3.14159265359, "Hello World"};
std::string result = applyFormat("Pi: %.3f, Text: %.5s", args);
// Result: "Pi: 3.142, Text: Hello"
```

## Error Handling

### Missing Arguments
When format specifiers exceed available arguments:
- Returns empty `QuantumValue()` for missing arguments
- Gracefully handles out-of-bounds access

### Invalid Specifiers
Unknown format specifiers are treated as literal text:
```cpp
applyFormat("Hello %Z", {}); // Returns "Hello %Z"
```

### Type Conversion
- Non-numeric arguments default to 0 for numeric specifiers
- String arguments converted using `toString()` method
- Boolean values handled by `isTruthy()` method

## Performance Considerations

### Buffer Management
- Uses fixed 256-character buffer for temporary formatting
- Avoids dynamic memory allocation during formatting
- Efficient string concatenation with `+=`

### Parsing Efficiency
- Single pass through format string
- Minimal branching in main loop
- Early termination for literal text

### Memory Usage
- Stack-allocated buffer prevents heap fragmentation
- Lambda functions for argument access avoid virtual calls
- Static function enables compiler optimizations

## Thread Safety

The function is thread-safe because it:
- Operates only on input parameters
- Uses only local variables
- Has no shared mutable state
- Uses thread-safe standard library functions

## Integration Points

### Built-in Functions
- `printf()`: Direct formatting to console
- `format()`: String formatting function
- `sprintf()`: String formatting with return value

### Method Calls
- String formatting methods
- Number formatting methods
- Custom formatting functions

## Best Practices

### Format String Design
- Use descriptive format strings
- Include appropriate precision for floating-point values
- Consider internationalization for localized output

### Argument Validation
- Ensure sufficient arguments for format specifiers
- Validate argument types when possible
- Handle edge cases (null values, empty strings)

### Performance Optimization
- Pre-allocate format strings when possible
- Avoid excessive precision for large datasets
- Consider caching frequently used format patterns

## Testing Considerations

### Test Categories
- Basic format specifier functionality
- Flag combinations and interactions
- Width and precision edge cases
- Quantum extension features
- Error handling and edge cases
- Performance with large datasets

### Edge Cases
- Empty format strings
- Format strings with only literals
- Very large width/precision values
- Negative precision values
- Unicode and multi-byte characters

## Summary

The `applyFormat()` function provides a comprehensive, high-performance formatting engine that combines C-style printf compatibility with Quantum-specific extensions. Its modular design, extensive feature set, and robust error handling make it a cornerstone of the Quantum Language's string formatting capabilities while maintaining compatibility with existing C formatting patterns.
