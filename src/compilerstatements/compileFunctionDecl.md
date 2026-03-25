# `compileFunctionDecl` Function

The `compileFunctionDecl` function is responsible for compiling a function declaration in the Quantum Language compiler. This function takes several parameters and performs various operations to generate bytecode that represents the function.

## Parameters/Return Value

- **Parameters**:
  - `s`: A structure representing the function declaration, which includes details such as the function name (`name`), parameters (`params`), whether each parameter is passed by reference (`paramIsRef`), default arguments (`defaultArgs`), and the body of the function (`body`).
  - `line`: The line number where the function declaration occurs in the source code.

- **Return Value**: None. The function compiles the function declaration and emits corresponding bytecode instructions.

## What It Does

The primary task of the `compileFunctionDecl` function is to compile a function declaration into bytecode. Here’s a step-by-step breakdown of what it does:

1. **Compile Function Body**:
   ```cpp
   auto fnChunk = compileFunction(s.name, s.params, s.paramIsRef, s.defaultArgs, s.body.get(), line);
   ```
   - Calls the `compileFunction` method to compile the function body into a chunk of bytecode (`fnChunk`). This chunk contains the instructions for executing the function's code.

2. **Create Closure Template**:
   ```cpp
   auto closureTpl = std::make_shared<Closure>(fnChunk);
   ```
   - Creates a shared pointer to a `Closure` object using the compiled function chunk. A closure captures variables from its surrounding scope.

3. **Load Constant**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(closureTpl)), line);
   ```
   - Emits an `Op::LOAD_CONST` instruction to load the constant value of the closure template onto the stack. This allows the closure to be used later.

4. **Make Function/Closure**:
   ```cpp
   emit(fnChunk->upvalueCount > 0 ? Op::MAKE_CLOSURE : Op::MAKE_FUNCTION, 0, line);
   ```
   - Depending on whether the function has upvalues (variables captured from outer scopes), it emits either an `Op::MAKE_CLOSURE` or an `Op::MAKE_FUNCTION` instruction. Upvalues require additional handling during closure creation.

5. **Define Global or Local Function**:
   ```cpp
   if (current_->scopeDepth == 0)
   {
       emit(Op::DEFINE_GLOBAL, addStr(s.name), line);
   }
   else
   {
       int existingSlot = resolveLocal(current_, s.name);
       if (existingSlot != -1 && current_->locals[existingSlot].depth == current_->scopeDepth)
       {
           emit(Op::STORE_LOCAL, existingSlot, line);
           emit(Op::POP, 0, line);
       }
       else
       {
           declareLocal(s.name, line);
           emit(Op::DEFINE_LOCAL, static_cast<int>(current_->locals.size()) - 1, line);
       }
   }
   ```
   - Checks the current scope depth to determine whether the function should be defined globally or locally.
     - If the scope depth is 0, it means the function is at the global level, so it emits an `Op::DEFINE_GLOBAL` instruction to define the function in the global namespace.
     - If the scope depth is greater than 0, it checks if there is already a local variable with the same name.
       - If found and the existing slot matches the current scope depth, it emits an `Op::STORE_LOCAL` instruction to store the function in the existing local slot and then pops it from the stack.
       - If not found or the existing slot does not match the current scope depth, it declares a new local variable and emits an `Op::DEFINE_LOCAL` instruction to define the function in the newly declared local slot.

## Why It Works This Way

This implementation ensures that functions are properly compiled and stored based on their scope. Functions at the global level are directly defined in the global namespace, while functions within nested scopes are handled as local variables. This approach leverages closures to manage state and variables from enclosing scopes, providing flexibility and encapsulation in the quantum language.

## Edge Cases

- **Global Function Declaration**: When declaring a function at the global level, ensure that the function name is unique to avoid conflicts with existing global functions.
- **Nested Function Declaration**: When declaring a function within a nested scope, check for existing local variables with the same name to prevent shadowing and ensure correct scoping.
- **Upvalues**: Handle upvalues appropriately to maintain the correct lexical environment when capturing variables from outer scopes.

## Interactions With Other Components

- **Scope Management**: The function interacts with the scope management system to track the depth of the current scope and manage local variables.
- **Bytecode Emission**: It uses the bytecode emission system to generate and emit instructions that represent the function declaration and its execution.
- **Constant Pool**: The function utilizes the constant pool to store constants, including the closure template, ensuring efficient memory usage and faster execution.

Overall, the `compileFunctionDecl` function plays a crucial role in compiling function declarations in the Quantum Language compiler, ensuring proper scoping, closure management, and bytecode generation.