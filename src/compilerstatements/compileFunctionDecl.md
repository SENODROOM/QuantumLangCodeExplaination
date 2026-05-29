# `compileFunctionDecl` Function

## Overview

The `compileFunctionDecl` function is a critical component of the Quantum Language compiler, responsible for converting a function declaration into executable bytecode. This process enables the correct invocation of functions throughout the program. The function handles various aspects such as compiling the function body, creating closures, and defining the function in the appropriate scope.

## Parameters

- `s`: A reference to a `FunctionDeclaration` object representing the function declaration to be compiled.
- `line`: An integer indicating the line number where the function declaration occurs in the source code.

## Return Value

This function does not explicitly return a value. Instead, it performs several operations that contribute to the overall compilation process:
- Compiles the function body.
- Creates a closure template.
- Emits bytecode instructions to load constants, define local or global variables, and create the function or closure.

## Edge Cases

1. **Default Arguments**: If the function has default arguments, they are handled during the compilation of the function body. The `defaultArgs` parameter ensures that these defaults are correctly applied when the function is invoked.
2. **Upvalues**: If the function uses upvalues (i.e., references to variables from an enclosing scope), the `fnChunk->upvalueCount` is checked to determine whether to emit the `Op::MAKE_CLOSURE` instruction instead of `Op::MAKE_FUNCTION`.
3. **Scope Depth**: The function checks the `current_->scopeDepth` to determine whether to define the function globally or locally. If the scope depth is zero, the function is defined globally using `Op::DEFINE_GLOBAL`. Otherwise, it is either stored in an existing local slot or declared as a new local variable.

## Interactions with Other Components

- **Compilation Context (`current_`)**: The function interacts with the current compilation context, which includes information about the current scope depth and local variables. This context is used to manage the scope and lifetime of variables.
- **Bytecode Emission (`emit`)**: The function emits bytecode instructions to represent the compiled function. These instructions include loading constants, defining variables, and creating functions or closures.
- **Closure Template (`Closure`)**: The function creates a closure template (`std::make_shared<Closure>(fnChunk)`) to encapsulate the function's environment and upvalues. This template is then loaded onto the stack and used to create the actual function or closure.
- **Constant Pool (`addConst`)**: The function adds constants to the constant pool using the `addConst` method. This includes adding the closure template to the pool.
- **String Pool (`addStr`)**: If the function is being defined globally, its name is added to the string pool using the `addStr` method. This allows for efficient storage and retrieval of string literals.

## Detailed Explanation

### Step-by-Step Breakdown

1. **Compile Function Body**:
   ```cpp
   auto fnChunk = compileFunction(s.name, s.params, s.paramIsRef, s.defaultArgs, s.body.get(), line);
   ```
   - This step compiles the function body into a chunk of bytecode (`fnChunk`). The `compileFunction` function takes the function's name, parameters, parameter types, default arguments, body, and line number as input.

2. **Create Closure Template**:
   ```cpp
   auto closureTpl = std::make_shared<Closure>(fnChunk);
   ```
   - After compiling the function body, a closure template is created. This template encapsulates the function's bytecode and any necessary upvalues.

3. **Load Constant**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(closureTpl)), line);
   ```
   - The closure template is loaded onto the stack as a constant using the `Op::LOAD_CONST` opcode. The `addConst` method adds the closure template to the constant pool.

4. **Make Function/Closure**:
   ```cpp
   emit(fnChunk->upvalueCount > 0 ? Op::MAKE_CLOSURE : Op::MAKE_FUNCTION, 0, line);
   ```
   - Depending on whether the function uses upvalues (`fnChunk->upvalueCount > 0`), the appropriate opcode is emitted to create the function or closure. If upvalues are present, `Op::MAKE_CLOSURE` is used; otherwise, `Op::MAKE_FUNCTION` is used.

5. **Define Global/Local Variable**:
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
   - Finally, the function defines itself in the appropriate scope:
     - If the scope depth is zero, it defines the function globally using `Op::DEFINE_GLOBAL` and the function's name from the string pool (`addStr(s.name)`).
     - If the scope depth is greater than zero, it resolves the local variable slot where the function should be stored:
       - If the variable already exists at the same scope depth, it stores the function in that slot and pops the closure from the stack.
       - If the variable does not exist, it declares a new local variable and defines the function in that slot.

By following these steps, the `compileFunctionDecl` function ensures that function declarations