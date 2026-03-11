# toInt() Function Documentation

## Overview

The `toInt()` function is a helper function in the Quantum Language interpreter that provides safe type conversion from `QuantumValue` objects to integer types. It builds upon the `toNum()` function to ensure consistent type checking and error handling throughout the interpreter.

## Function Signature

```cpp
static long long toInt(const QuantumValue &v, const std::string &ctx)
```

## Parameters

- **`v`** (`const QuantumValue &`): The input value to be converted to an integer
- **`ctx`** (`const std::string &`): Context string for error reporting, typically describing the operation being performed

## Return Value

- **`long long`**: The integer representation of the input value
- **Throws**: `TypeError` if the input value cannot be converted to a number

## Implementation

```cpp
static long long toInt(const QuantumValue &v, const std::string &ctx)
{
    return (long long)toNum(v, ctx);
}
```

## Behavior

### Conversion Process
1. **Delegates to `toNum()`**: The function calls `toNum(v, ctx)` to perform the initial type checking and numeric conversion
2. **Casts to Integer**: The resulting `double` from `toNum()` is cast to `long long`
3. **Error Propagation**: Any `TypeError` from `toNum()` is propagated to the caller

### Type Conversion Rules
- **Integer values**: Preserved exactly (no loss of precision)
- **Float values**: Truncated toward zero (standard C++ casting behavior)
- **Large numbers**: May overflow if outside `long long` range
- **Non-numeric types**: Rejected with `TypeError`

## Usage Contexts

The `toInt()` function is used throughout the interpreter in contexts requiring integer values:

### Array Indexing
```cpp
// When accessing array elements with integer indices
int index = (int)toInt(indexExpr, "array indexing");
```

### String Operations
```cpp
// For string slicing and character positions
int start = (int)toInt(args[0], "string slicing");
int length = (int)toInt(args[1], "string slicing");
```

### Loop Control
```cpp
// In for-loop bounds and iteration counts
long long start = toInt(startExpr, "for loop initialization");
long long end = toInt(endExpr, "for loop bounds");
```

### Bitwise Operations
```cpp
// When performing bitwise AND, OR, XOR operations
long long left = toInt(leftExpr, "bitwise AND operation");
long long right = toInt(rightExpr, "bitwise AND operation");
```

### Mathematical Functions
```cpp
// For functions requiring integer parameters
long long base = toInt(args[0], " logarithm base");
```

## Design Rationale

### Code Reuse
By delegating to `toNum()`, the function:
- Avoids duplicating type checking logic
- Ensures consistent error messages
- Maintains single source of truth for numeric conversion

### Type Safety
- Provides explicit integer conversion with clear intent
- Prevents implicit floating-point to integer conversions
- Maintains strong typing throughout the codebase

### Performance
- Minimal overhead: just a function call and cast
- `static` qualifier enables compiler optimizations
- No additional memory allocation

## Error Handling

### Error Types
The function propagates `TypeError` from `toNum()` when:
- Input value is not numeric (string, array, object, etc.)
- Input value is `null` or `undefined`

### Error Message Format
```
Expected number in [context], got [actual_type]
```

### Example Error Messages
- `"Expected number in array indexing, got string"`
- `"Expected number in loop bounds, got object"`

## Conversion Behavior

### From Integer Types
```cpp
QuantumValue intValue(42);
long long result = toInt(intValue, "test"); // result = 42
```

### From Float Types
```cpp
QuantumValue floatValue(3.14159);
long long result = toInt(floatValue, "test"); // result = 3 (truncated)
```

### From Negative Floats
```cpp
QuantumValue floatValue(-2.7);
long long result = toInt(floatValue, "test"); // result = -2 (truncated toward zero)
```

## Edge Cases

### Large Numbers
```cpp
QuantumValue largeValue(1e20); // May overflow long long
long long result = toInt(largeValue, "test"); // Implementation-defined behavior
```

### Infinity and NaN
```cpp
QuantumValue infinity(std::numeric_limits<double>::infinity());
long long result = toInt(infinity, "test"); // Implementation-defined behavior
```

## Relationship to toNum()

### Dependency
`toInt()` is a thin wrapper around `toNum()`:
- `toNum()` handles type validation and initial conversion
- `toInt()` adds the integer casting step

### Consistency
Both functions share:
- Same error handling approach
- Same context parameter usage
- Same type checking logic

## Performance Considerations

### Function Call Overhead
- One function call to `toNum()`
- One cast operation
- Minimal stack usage

### Compiler Optimizations
- `static` functions can be inlined
- Cast operations are optimized by compiler
- No runtime type information overhead

## Thread Safety

The function is thread-safe because it:
- Operates only on input parameters
- Uses only thread-safe operations
- Has no shared mutable state

## Best Practices

### Context Naming
Use descriptive context names:
- ✅ `"array indexing"`
- ✅ `"string length calculation"`
- ✅ `"loop iteration count"`
- ❌ `"conversion"`
- ❌ `"operation"`

### Error Handling
```cpp
try {
    long long index = toInt(value, "array access");
    // Use index safely
} catch (const TypeError& e) {
    // Handle type error appropriately
}
```

### Range Validation
For critical applications, consider additional range validation:
```cpp
long long index = toInt(value, "array access");
if (index < 0 || index >= arraySize) {
    throw RuntimeError("Array index out of bounds");
}
```

## Testing Considerations

### Test Cases
- Valid integer inputs
- Valid float inputs (verify truncation behavior)
- Invalid non-numeric inputs
- Edge cases (very large numbers, infinity, NaN)
- Negative numbers
- Zero values

### Error Testing
- Verify error messages are descriptive
- Test context propagation
- Ensure exceptions are properly thrown

## Example Usage

```cpp
// Example: Array indexing implementation
QuantumValue evalIndex(IndexExpr& expr) {
    QuantumValue array = evaluate(expr.object);
    QuantumValue indexValue = evaluate(expr.index);
    
    try {
        long long index = toInt(indexValue, "array indexing");
        
        if (!array.isArray()) {
            throw TypeError("Cannot index non-array type");
        }
        
        auto& arr = *array.asArray();
        if (index < 0 || index >= (long long)arr.size()) {
            throw RuntimeError("Array index out of bounds");
        }
        
        return arr[index];
    } catch (const TypeError& e) {
        throw; // Re-throw with original context
    }
}
```

## Comparison with Direct Casting

### Before (unsafe)
```cpp
long long index = (long long)value.asNumber(); // No type checking
```

### After (safe)
```cpp
long long index = toInt(value, "array indexing"); // Type-safe with errors
```

## Summary

The `toInt()` function provides a type-safe, consistent way to convert `QuantumValue` objects to integers throughout the Quantum Language interpreter. By building upon `toNum()`, it ensures uniform error handling and type checking while providing the specific integer conversion needed for indexing, loop control, and other integer-specific operations.
