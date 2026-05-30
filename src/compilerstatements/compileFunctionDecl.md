# `compileFunctionDecl` Function

## Overview

The `compileFunctionDecl` function is a crucial component of the Quantum Language compiler, responsible for converting a function declaration into executable bytecode. This process ensures that functions can be invoked correctly throughout the program.

### Parameters
- `s`: A reference to a `FunctionDeclaration` object representing the function to be compiled.
- `line`: An integer indicating the line number in the source code where the function declaration occurs.

### Return Value
This function does not explicitly return a value. Instead, it performs several operations to compile and define the function within the bytecode.

### Edge Cases
1. **Global Functions**: If the current scope depth (`current_->scopeDepth`) is zero, the function is defined as a global function using the `Op::DEFINE_GLOBAL` opcode. This allows the function to be accessible from anywhere in the program.
2. **Nested Functions**: For nested functions (where `current_->scopeDepth` is greater than zero), the function is defined locally using the `Op::DEFINE_LOCAL` opcode. This ensures that the function is only accessible within its defining scope.
3. **Parameter Types**: The function handles both regular and reference parameters (`s.paramIsRef`). Reference parameters are managed differently during compilation to ensure proper memory handling.
4. **Default Arguments**: Default arguments are processed before the main body of the function is compiled. This allows them to be included in the function's signature and used appropriately within the function.

### Interactions with Other Components
- **Bytecode Emission**: The function interacts with the bytecode emission system through calls to `emit`, which adds opcodes to the bytecode stream. These opcodes include loading constants, making closures or functions, and defining local/global variables.
- **Scope Management**: It uses the `current_` pointer to access the current scope information. This includes managing local variables and determining whether a function should be defined globally or locally based on the scope depth.
- **Closure Creation**: When compiling a function with upvalues (closure), it creates a `Closure` template and emits the appropriate opcode (`Op::MAKE_CLOSURE`) to handle these upvalues.
- **Constant Pool**: The function utilizes the constant pool to store constants such as the function chunk and string representations of variable names. This helps in reducing redundancy and improving performance during execution.

### Detailed Explanation
1. **Compilation of Function Body**:
   ```cpp
   auto fnChunk = compileFunction(s.name, s.params, s.paramIsRef, s.defaultArgs, s.body.get(), line);
   ```
   - This line compiles the main body of the function specified by `s`. The result is stored in `fnChunk`, which represents the bytecode chunk for the function.

2. **Creating Closure Template**:
   ```cpp
   auto closureTpl = std::make_shared<Closure>(fnChunk);
   ```
   - If the function has upvalues, a `Closure` template is created. This template encapsulates the function chunk along with any necessary upvalues.

3. **Emitting Load Constant Opcode**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(closureTpl)), line);
   ```
   - The function loads the constant representation of the closure template onto the stack. This step is essential for subsequent operations that involve invoking the function.

4. **Making Function or Closure**:
   ```cpp
   emit(fnChunk->upvalueCount > 0 ? Op::MAKE_CLOSURE : Op::MAKE_FUNCTION, 0, line);
   ```
   - Based on whether the function has upvalues, the appropriate opcode (`Op::MAKE_CLOSURE` or `Op::MAKE_FUNCTION`) is emitted. This opcode prepares the function or closure for execution.

5. **Defining Global or Local Function**:
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
   - Depending on the scope depth, the function is either defined as a global or a local function.
     - **Global Function**: If the scope depth is zero, the function is defined globally using the `Op::DEFINE_GLOBAL` opcode.
     - **Local Function**: For nested functions, the function is declared locally using the `declareLocal` function. Then, it is defined locally using the `Op::DEFINE_LOCAL` opcode.

By following these steps, the `compileFunctionDecl` function ensures that each function declaration in the Quantum Language is accurately converted into bytecode, allowing for efficient execution and proper function scoping within the program.