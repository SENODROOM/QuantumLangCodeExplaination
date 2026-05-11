# `define` Function in Quantum Language Compiler

## Overview
The `define` function in the Quantum Language compiler is crucial for introducing new variables or constants into the current lexical scope. This function updates the symbol table, ensuring that these identifiers can be accessed and used throughout the program. By defining variables and constants, the compiler facilitates data management and control flow, making it possible to write complex quantum algorithms efficiently.

## Parameters
- **name**: A string representing the name of the variable or constant being defined.
- **val**: The value associated with the variable or constant. This could be any type supported by the Quantum Language, such as integers, floating-point numbers, or complex types like qubits.
- **isConst**: A boolean flag indicating whether the identifier should be treated as a constant. If set to `true`, the identifier's value cannot be modified after its initial assignment.

## Return Value
This function does not return any value explicitly. However, it modifies the internal state of the compiler by updating the symbol table with the new variable or constant.

## Edge Cases
1. **Duplicate Names**: If an attempt is made to define a variable or constant with a name that already exists in the current scope, the existing definition will be overwritten. This behavior ensures that there is only one definition per identifier at any given time.
2. **Scope Management**: The `define` function operates on the current lexical scope. If called outside of a valid scope, it may lead to undefined behavior or errors.
3. **Constant Assignment**: When defining a constant (`isConst` set to `true`), attempting to reassign a new value to the constant will result in an error. This enforces immutability and helps prevent logical errors in the code.

## Interactions with Other Components
- **Symbol Table**: The `define` function interacts directly with the symbol table, which is responsible for storing information about all variables and constants in the program. It adds the new identifier to the symbol table along with its value and mutability status.
- **Error Handling**: During the execution of the `define` function, the compiler checks for duplicate names and immutable assignments. If either condition is violated, appropriate error messages are generated and handled, preventing the program from running with incorrect definitions.

## Example Usage
```cpp
// Define a variable 'x' with an integer value 5
define("x", 5);

// Define a constant 'pi' with a floating-point value 3.14159
define("pi", 3.14159, true);

// Attempting to redefine 'x' will overwrite the existing value
define("x", 10); // Now 'x' has the value 10

// Attempting to reassign 'pi' will result in an error since it is a constant
define("pi", 3.14); // Error: Cannot reassign constant 'pi'
```

By understanding how the `define` function works and its interactions with other components, developers can effectively manage their quantum programs, ensuring correct data handling and preventing runtime errors.