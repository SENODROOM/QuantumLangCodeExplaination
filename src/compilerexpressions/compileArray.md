# `compileArray` Function

## Purpose
The `compileArray` function in the Quantum Language compiler is responsible for compiling array literals into executable bytecode. This function handles both regular arrays and those containing spread operators (`...`), which allow an iterable to be expanded within the array literal.

## Parameters/Return Value
- **Parameters**:
  - `e`: A reference to an `ArrayExpr` object representing the array literal to be compiled.
- **Return Value**: None. The function emits bytecode directly using the `emit` function.

## How It Works
1. **Check for Spread Operator**:
   - The function first checks if the array contains any spread operators (`...`). This is done by iterating through each element of the array and checking if it is a unary expression with the operator set to `"..."`.

2. **Handling Arrays with Spread Operators**:
   - If a spread operator is found, the function starts by emitting the `Op::MAKE_ARRAY` instruction with a size parameter of `0`. This creates an empty array on the stack.
   - For each element in the array, the function determines whether it is a spread operator or not.
     - If it is a spread operator, it emits the `Op::LOAD_GLOBAL` instruction to load the `__array_extend__` function from the global scope. This function is designed to handle the expansion of iterables.
     - If it is not a spread operator, it emits the `Op::LOAD_GLOBAL` instruction to load the `__listcomp_push__` function from the global scope. This function is used to push elements onto the array.
   - After loading the appropriate function, the function swaps the top two values on the stack (the array and the element) using the `Op::SWAP` instruction.
   - Depending on whether the current element is a spread operator, it either compiles the operand of the spread operator or the element itself.
   - Finally, the function calls the loaded function using the `Op::CALL` instruction with a parameter of `2`, indicating that it expects two arguments (the array and the element).

3. **Handling Regular Arrays**:
   - If no spread operators are found, the function simply iterates through each element of the array and compiles them one by one.
   - After all elements have been compiled, the function emits the `Op::MAKE_ARRAY` instruction with a size parameter equal to the number of elements in the array. This creates an array on the stack containing all the compiled elements.

## Edge Cases
- **Empty Array**: If the array is empty, the function will still emit the `Op::MAKE_ARRAY` instruction with a size parameter of `0`, resulting in an empty array being pushed onto the stack.
- **Nested Arrays**: The function can handle nested arrays as long as they do not contain spread operators. Each nested array will be compiled separately and then added to the outer array.
- **Spread Operator with Non-Iterable**: If a spread operator is used with a non-iterable element, the compilation process may fail or produce unexpected results. The `__array_extend__` function should handle such cases gracefully.

## Interactions with Other Components
- **Emit Function**: The `compileArray` function interacts with the `emit` function to generate bytecode instructions. The `emit` function takes three parameters: the operation code, the number of operands, and the line number where the operation occurs.
- **Global Scope Functions**: The function loads and uses global functions like `__array_extend__` and `__listcomp_push__` to handle spread operators and pushing elements onto the array, respectively. These functions are part of the quantum language's runtime environment and must be defined elsewhere in the compiler or interpreter.

By handling both regular arrays and arrays with spread operators, the `compileArray` function ensures flexibility in array construction while maintaining efficient bytecode generation.