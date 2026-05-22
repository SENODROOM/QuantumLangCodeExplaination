# `execBinary`

The `execBinary` function is an essential part of the Quantum Language compiler's virtual machine core (`VmCore.cpp`). It is responsible for executing binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). The function ensures that operations are performed correctly across different data types, including strings, arrays, numbers, and booleans.

## Parameters

- **`L`**: A `QuantumValue` representing the left operand in the binary operation.
- **`R`**: A `QuantumValue` representing the right operand in the binary operation.
- **`op`**: An `Op` enum indicating the type of binary operation to be executed.

## Return Value

- Returns a `QuantumValue` representing the result of the binary operation.

## How it Works

### String Concatenation

If either `L` or `R` is a string, and the operation type is `Op::ADD`, the function concatenates the two operands into a single string and returns it as a `QuantumValue`.

```cpp
if (op == Op::ADD && (L.isString() || R.isString()))
    return QuantumValue(L.toString() + R.toString());
```

### Array Concatenation

If both `L` and `R` are arrays, and the operation type is `Op::ADD`, the function creates a new array by copying elements from `L` and appending those from `R`. The resulting array is then returned as a `QuantumValue`.

```cpp
if (op == Op::ADD && L.isArray() && R.isArray())
{
    auto arr = std::make_shared<Array>(*L.asArray());
    for (auto &v : *R.asArray())
        arr->push_back(v);
    return QuantumValue(arr);
}
```

### Comparison Operators

For comparison operations (`Op::EQ` and `Op::NEQ`), the function allows mixed types. It compares the values and returns a boolean `QuantumValue` indicating whether the condition is true or false.

```cpp
if (op == Op::EQ)
    return QuantumValue(valuesEqual(L, R));
if (op == Op::NEQ)
    return QuantumValue(!valuesEqual(L, R));
```

### Numeric Arithmetic

If both `L` and `R` are numeric types, the function performs the corresponding arithmetic operation (`Op::ADD`, `Op::SUB`, `Op::MUL`, etc.) and returns the result as a `QuantumValue`.

For non-numeric types, the function attempts to convert them to numbers:

- If `L` or `R` is a string, it tries to convert the string to a double using `std::stod`. If conversion fails, it defaults to 0.
- If `L` or `R` is a boolean, it converts `true` to 1.0 and `false` to 0.0.

After converting both operands to numbers, the function executes the arithmetic operation and returns the result.

```cpp
double l = 0, r = 0;
bool hasNum = L.isNumber() || R.isNumber();

if (L.isNumber())
    l = L.asNumber();
else if (L.isString())
{
    try
    {
        l = std::stod(L.asString());
    }
    catch (...)
    {
        l = 0;
    }
}
else if (L.isBool())
    l = L.asBool() ? 1.0 : 0.0;

if (R.isNumber())
    r = R.asNumber();
else if (R.isString())
{
    try
    {
        r = std::stod(R.asString());
    }
    catch (...)
    {
        r = 0;
    }
}
else if (R.isBool())
    r = R.asBool() ? 1.0 : 0.0;

switch (op)
{
case Op::ADD:
    return QuantumValue(l + r);
case Op::SUB:
    return QuantumValue(l - r);
case Op::MUL:
    return QuantumValue(l * r);
// Additional cases for other arithmetic operations
}
```

## Edge Cases

- **Type Mismatch**: When performing arithmetic operations, if one operand is a string and the other is not, the string is converted to a number before the operation. If the string cannot be converted to a valid number, it defaults to 0.
- **Mixed Types**: For comparison operations, the function handles mixed types gracefully by converting them to numbers where possible.
- **Negative Numbers**: When dealing with numeric operations involving negative numbers, the function follows standard arithmetic rules.

## Interactions with Other Components

- **`QuantumValue` Class**: The `execBinary` function interacts with the `QuantumValue` class to handle different data types and perform operations.
- **`Op` Enum**: The function uses the `Op` enum to determine which binary operation to execute.
- **Error Handling**: The function includes error handling mechanisms to manage situations where conversions fail, ensuring robustness in the virtual machine core.

Overall, the `execBinary` function plays a critical role in the Quantum Language compiler's virtual machine core by enabling the execution of various binary operations across different data types. Its design ensures flexibility and correctness, making it a fundamental component of the compiler's functionality.