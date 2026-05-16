# `compileFunctionDecl` Function

## Overview

The `compileFunctionDecl` function is a crucial component of the Quantum Language compiler, tasked with converting a function declaration into executable bytecode. This transformation facilitates the correct invocation of functions within the program's scope.

## Parameters and Return Value

- **Parameters**:
  - `s.name`: A string representing the name of the function to be compiled.
  - `s.params`: A vector of strings representing the names of the function parameters.
  - `s.paramIsRef`: A boolean indicating whether each parameter is passed by reference.
  - `s.defaultArgs`: An optional vector of default argument values for the function.
  - `s.body.get()`: A pointer to the body of the function, which contains the statements to be executed.
  - `line`: The line number in the source code where the function declaration occurs.

- **Return Value**: None. The function directly emits bytecode instructions to the current compilation context.

## How It Works

1. **Compilation of Function Body**:
   - The function body (`s.body`) is compiled using the `compileFunction` method, resulting in a `FunctionChunk` object (`fnChunk`). This chunk encapsulates the bytecode generated from the function's body.

2. **Creating Closure Template**:
   - A `Closure` template is created using the `std::make_shared` function, passing the `fnChunk` as its parameter. This closure will hold the function's bytecode and any upvalues it might need.

3. **Loading Constant**:
   - The `emit` function is used to load a constant onto the stack. The constant is the `Closure` template created in the previous step. This is done using the `Op::LOAD_CONST` opcode and the index obtained from `addConst`.

4. **Making Function or Closure**:
   - Depending on whether the `fnChunk` has upvalues (`fnChunk->upvalueCount > 0`), the `emit` function uses either the `Op::MAKE_CLOSURE` or `Op::MAKE_FUNCTION` opcode to create the function or closure. The second argument to `emit` is set to `0`, indicating that there are no additional arguments required for these opcodes.

5. **Global vs Local Declaration**:
   - If the current scope depth is `0` (`current_->scopeDepth == 0`), the function is declared globally using the `Op::DEFINE_GLOBAL` opcode. The function name is added to the constants table using `addStr`.
   - If the current scope depth is not `0`, the function attempts to resolve whether the function already exists in the local scope at the same depth using the `resolveLocal` method. If found, it stores the function in the local slot and pops it from the stack. Otherwise, it declares the function locally using the `declareLocal` method and defines it in the local scope using the `Op::DEFINE_LOCAL` opcode.

6. **Edge Cases**:
   - **Parameter Types**: The function handles both regular and reference parameters, ensuring that the appropriate handling is applied during the compilation process.
   - **Default Arguments**: If default arguments are provided, they are included in the function's definition, allowing for flexible function calls.
   - **Scope Management**: The function manages the scope depth effectively, distinguishing between global and local declarations based on the current scope depth.

## Interactions with Other Components

- **Constants Table**: The `addConst` function interacts with the constants table to store the `Closure` template as a constant, making it available for use in subsequent bytecode instructions.
  
- **Locals Table**: The `resolveLocal` and `declareLocal` methods interact with the locals table to manage local variables and their slots within the current scope.

- **Bytecode Emission**: The `emit` function is used throughout the process to generate and emit bytecode instructions into the current compilation context.

- **Scope Depth Tracking**: The `current_->scopeDepth` variable tracks the depth of the current scope, influencing decisions about whether a function should be defined globally or locally.

This comprehensive approach ensures that function declarations are accurately transformed into executable bytecode, maintaining the integrity and functionality of the quantum program being compiled.