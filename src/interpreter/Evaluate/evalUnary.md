# evalUnary() Function Explanation

## Complete Code

```cpp
QuantumValue Interpreter::evalUnary(UnaryExpr &e)
{
    auto v = evaluate(*e.operand);
    if (e.op == "-")
        return QuantumValue(-v.toNum("unary -"));
    if (e.op == "!")
        return QuantumValue(!v.isTruthy());
    if (e.op == "~")
        return QuantumValue(~(long long)v.toNum("unary ~"));
    if (e.op == "++")
    {
        setLValue(*e.operand, v + 1, "=");
        return v;
    }
    if (e.op == "--")
    {
        setLValue(*e.operand, v - 1, "=");
        return v;
    }
    throw RuntimeError("Unknown unary op: " + e.op);
}
```

## Line-by-Line Explanation

### Function Signature (Line 3002)
- **Line 3002**: `QuantumValue Interpreter::evalUnary(UnaryExpr &e)` - Evaluate unary expressions
  - `e`: Reference to UnaryExpr AST node
  - Returns QuantumValue result of unary operation

### Operand Evaluation (Lines 3003-3004)
- **Line 3003**: `{` - Opening brace
- **Line 3004**: `auto v = evaluate(*e.operand);` - Evaluate operand expression

### Negation Operator (Lines 3005-3007)
- **Line 3005**: `if (e.op == "-")` - Check for negation operator
- **Line 3006**: `return QuantumValue(-v.toNum("unary -"));` - Return numeric negation
- **Line 3007**: Empty line for readability

### Logical Not Operator (Lines 3008-3010)
- **Line 3008**: `if (e.op == "!")` - Check for logical not operator
- **Line 3009**: `return QuantumValue(!v.isTruthy());` - Return logical negation
- **Line 3010**: Empty line for readability

### Bitwise Not Operator (Lines 3011-3013)
- **Line 3011**: `if (e.op == "~")` - Check for bitwise not operator
- **Line 3012**: `return QuantumValue(~(long long)v.toNum("unary ~"));` - Return bitwise complement
- **Line 3013**: Empty line for readability

### Pre-increment Operator (Lines 3014-3019)
- **Line 3014**: `if (e.op == "++")` - Check for pre-increment operator
- **Line 3015**: `{` - Opening brace for increment
- **Line 3016**: `setLValue(*e.operand, v + 1, "=");` - Increment operand and assign
- **Line 3017**: `return v;` - Return original value (pre-increment semantics)
- **Line 3018**: `}` - Closing brace for increment
- **Line 3019**: Empty line for readability

### Pre-decrement Operator (Lines 3020-3025)
- **Line 3020**: `if (e.op == "--")` - Check for pre-decrement operator
- **Line 3021**: `{` - Opening brace for decrement
- **Line 3022**: `setLValue(*e.operand, v - 1, "=");` - Decrement operand and assign
- **Line 3023**: `return v;` - Return original value (pre-decrement semantics)
- **Line 3024**: `}` - Closing brace for decrement
- **Line 3025**: Empty line for readability

### Error Handling (Lines 3026-3028)
- **Line 3026**: `throw RuntimeError("Unknown unary op: " + e.op);` - Throw error for unknown operator
- **Line 3027**: `}` - Closing brace for function

## Summary

The `evalUnary()` function handles all unary operations in the Quantum Language:

### Key Features
- **Multiple Operators**: Supports arithmetic, logical, bitwise, and increment/decrement
- **Type Safety**: Proper type conversion and validation
- **Side Effects**: Increment/decrement operators modify operands
- **Error Handling**: Clear error messages for unknown operators

### Unary Operators Supported
- **Negation**: `-value` - numeric negation
- **Logical Not**: `!value` - truthiness negation
- **Bitwise Not**: `~value` - bitwise complement
- **Pre-increment**: `++value` - increment and return original
- **Pre-decrement**: `--value` - decrement and return original

### Operator Semantics
- **Arithmetic Negation**: Converts to number and negates
- **Logical Not**: Uses truthiness rules for negation
- **Bitwise Not**: Converts to integer and complements bits
- **Increment/Decrement**: Modifies variable and returns original value

### Type Conversion
- **Numeric Operations**: Convert operands to numbers
- **Logical Operations**: Use truthiness without conversion
- **Bitwise Operations**: Convert to integer for bit manipulation
- **Type Safety**: Proper error handling for invalid types

### Side Effect Handling
- **L-Value Assignment**: Uses setLValue for variable modification
- **Original Value Return**: Pre-increment/decrement return original value
- **Variable Update**: Variables are updated in-place
- **Scope Awareness**: Operates on variables in current scope

### Design Benefits
- **Comprehensive**: Covers all common unary operators
- **Type Safe**: Proper conversion and validation
- **Correct Semantics**: Follows language specification
- **Efficient**: Minimal overhead for operations

### Use Cases
- **Mathematical Operations**: `-x` for negation
- **Boolean Logic**: `!condition` for logical negation
- **Bit Manipulation**: `~mask` for bitwise complement
- **Variable Updates**: `++counter`, `--index` for increment/decrement

### Integration with Other Components
- **setLValue Function**: Used for variable modification
- **toNum Method**: Converts values to numbers
- **isTruthy Method**: Determines truthiness
- **Environment System**: Variables accessed through environment

### Performance Characteristics
- **Single Evaluation**: Operand evaluated once
- **Direct Operations**: Minimal overhead for simple operations
- **Type Conversion**: Efficient conversion when needed
- **Memory Efficient**: No unnecessary allocations

### Operator Examples
- **Negation**: `-5` → `5`, `-3.14` → `3.14`
- **Logical Not**: `!true` → `false`, `!0` → `true`
- **Bitwise Not**: `~5` → `-6`, `~0xFF` → `0x00`
- **Increment**: `++x` increments x, returns old value
- **Decrement**: `--x` decrements x, returns old value

### Error Handling
- **Type Errors**: Handled by toNum conversion
- **Unknown Operators**: RuntimeError with operator name
- **Variable Errors**: setLValue handles assignment errors
- **Conversion Errors**: Descriptive error messages

This function provides the foundation for unary operations in the Quantum Language, enabling mathematical, logical, and bitwise operations while maintaining proper type safety and side effect handling for increment/decrement operators.
