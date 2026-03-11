# toNum() Function Documentation

## Overview

The `toNum()` function is a critical helper function in the Quantum Language interpreter that provides safe type conversion from `QuantumValue` objects to numeric types. It serves as a fundamental building block for arithmetic operations and mathematical computations throughout the interpreter.

## Function Signature

```cpp
static double toNum(const QuantumValue &v, const std::string &ctx)
```

## Parameters

- **`v`** (`const QuantumValue &`): The input value to be converted to a number
- **`ctx`** (`const std::string &`): Context string for error reporting, typically describing the operation being performed

## Return Value

- **`double`**: The numeric representation of the input value
- **Throws**: `TypeError` if the input value cannot be converted to a number

## Implementation

```cpp
static double toNum(const QuantumValue &v, const std::string &ctx)
{
    if (v.isNumber())
        return v.asNumber();
    throw TypeError("Expected number in " + ctx + ", got " + v.typeName());
}
```

## Behavior

### Success Case
When the input `QuantumValue` contains a numeric type (integer, float, etc.):
1. The function checks `v.isNumber()` which returns `true`
2. It extracts the numeric value using `v.asNumber()`
3. Returns the value as a `double`

### Error Case
When the input `QuantumValue` contains a non-numeric type (string, array, object, etc.):
1. The function checks `v.isNumber()` which returns `false`
2. Throws a `TypeError` with a descriptive error message including:
   - The context where the error occurred
   - The actual type of the value that was provided

## Error Message Format

```
Expected number in [context], got [actual_type]
```

### Examples of Error Messages
- `"Expected number in addition, got string"`
- `"Expected number in array indexing, got object"`
- `"Expected number in function call, got array"`

## Usage Contexts

This function is used throughout the interpreter in various contexts:

### Arithmetic Operations
```cpp
// In binary operations like addition, subtraction, etc.
double left = toNum(leftExpr, "addition");
double right = toNum(rightExpr, "addition");
```

### Array Indexing
```cpp
// When accessing array elements with numeric indices
int index = (int)toNum(indexExpr, "array indexing");
```

### Function Arguments
```cpp
// When validating numeric function parameters
double angle = toNum(args[0], "sin function call");
```

### Loop Control
```cpp
// In for-loop condition evaluation
double start = toNum(startExpr, "for loop initialization");
```

## Design Rationale

### Type Safety
The function enforces strict type checking, preventing silent type coercion that could lead to unexpected behavior or runtime errors.

### Error Reporting
The context parameter provides meaningful error messages that help developers identify exactly where type mismatches occur in their code.

### Performance
The function is marked as `static` to allow compiler optimizations and avoid unnecessary function call overhead.

### Consistency
Using a centralized conversion function ensures consistent behavior across all numeric operations in the interpreter.

## Related Functions

### `toInt()` Function
```cpp
static long long toInt(const QuantumValue &v, const std::string &ctx)
{
    return (long long)toNum(v, ctx);
}
```

The `toInt()` function builds upon `toNum()` by casting the result to `long long`, providing integer-specific conversion.

## Type System Integration

The `toNum()` function works with the Quantum Language's dynamic type system:

- **Integer values**: Converted to double representation
- **Float values**: Returned as-is
- **Other types**: Rejected with TypeError

## Error Handling Best Practices

### Context Naming
When calling `toNum()`, use descriptive context names:
- ✅ `"array indexing"`
- ✅ `"addition operation"`
- ✅ `"function parameter validation"`
- ❌ `"operation"`
- ❌ `"conversion"`

### Error Recovery
Since `toNum()` throws exceptions, calling code should be prepared to handle `TypeError` exceptions appropriately, either by:
- Letting them propagate to the user
- Catching and re-throwing with additional context
- Providing alternative execution paths

## Performance Considerations

- The function performs minimal overhead - just a type check and value extraction
- `static` qualifier allows for potential compiler optimizations
- No memory allocation occurs during successful conversion
- Exception throwing only happens on error conditions

## Thread Safety

The function is thread-safe as it:
- Operates only on its parameters (no shared state)
- Uses only const member functions
- Performs no mutable operations

## Testing Considerations

When testing code that uses `toNum()`, ensure coverage of:
- Valid numeric inputs (integers, floats)
- Invalid non-numeric inputs (strings, arrays, objects)
- Error message accuracy and clarity
- Edge cases (NaN, infinity, very large numbers)

## Example Usage

```cpp
// Example: Implementing addition
QuantumValue evalAddition(BinaryExpr& expr) {
    QuantumValue left = evaluate(expr.left);
    QuantumValue right = evaluate(expr.right);
    
    try {
        double leftNum = toNum(left, "addition");
        double rightNum = toNum(right, "addition");
        return QuantumValue(leftNum + rightNum);
    } catch (const TypeError& e) {
        // Re-throw with additional context if needed
        throw;
    }
}
```

## Summary

The `toNum()` function is a fundamental utility that ensures type safety in numeric operations throughout the Quantum Language interpreter. Its simple yet effective design provides clear error messages and maintains consistency across the codebase, making it an essential component of the interpreter's type system.
