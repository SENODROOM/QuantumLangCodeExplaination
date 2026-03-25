# `compileClassDecl` Function

## Purpose
The `compileClassDecl` function in the Quantum Language compiler is responsible for compiling class declarations into bytecode. This includes creating the class object, handling inheritance, and defining methods within the class.

## Parameters/Return Value
- **Parameters**:
  - `s`: A reference to a `ClassDecl` object representing the class declaration to be compiled.
  - `line`: An integer representing the line number of the class declaration in the source code.

- **Return Value**: None. The function directly emits bytecode instructions using the `emit` function.

## Detailed Explanation

### Step-by-Step Breakdown

1. **Emit Class Name Constant**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(s.name)), line);
   ```
   - This instruction loads the constant string representation of the class name (`s.name`) onto the stack.
   - `addConst` adds the constant to the constant pool and returns its index.
   - `Op::LOAD_CONST` is an opcode that indicates loading a constant value onto the stack.

2. **Create Class Object**:
   ```cpp
   emit(Op::MAKE_CLASS, 0, line);
   ```
   - This instruction creates a new class object based on the loaded class name.
   - `Op::MAKE_CLASS` is an opcode that constructs a new class object.

3. **Handle Inheritance**:
   ```cpp
   if (!s.base.empty())
   {
       emitLoad(s.base, line);
       emit(Op::INHERIT, 0, line);
   }
   ```
   - If the class has a base class (`!s.base.empty()`), the following steps are executed:
     - `emitLoad(s.base, line)`: Loads the constant string representation of the base class name onto the stack.
     - `emit(Op::INHERIT, 0, line)`: Emits an opcode to inherit the current class from the base class.
   - `Op::INHERIT` is an opcode that sets up inheritance between two classes.

4. **Define Methods**:
   ```cpp
   for (auto &method : s.methods)
   {
       if (!method->is<FunctionDecl>())
           continue;
       auto &fd = method->as<FunctionDecl>();
       // Prepend "self" as slot 0 so the instance is always at the first local.
       // The VM calls methods with the instance as the first argument (argCount+1).
       // "this" references are resolved to "self" by emitLoad/emitStore.
       std::vector<std::string> methodParams;
       std::vector<bool> methodRefs;
       methodParams.push_back("self");
       methodRefs.push_back(false);
       for (size_t i = 0; i < fd.params.size(); ++i)
       {
           methodParams.push_back(fd.params[i]);
           methodRefs.push_back(i < fd.paramIsRef.size() ? fd.paramIsRef[i] : false);
       }

       auto fnChunk = compileFunction(fd.name, methodParams, methodRefs, fd.defaultArgs, fd.body.get(), method->line);
       auto closureTpl = std::make_shared<Closure>(fnChunk);
       emit(Op::LOAD_CONST, addConst(QuantumValue(closureTpl)), method->line);
       emit(Op::MAKE_FUNCTION, 0, method->line);
       emit(Op::BIND_METHOD, addStr(fd.name), method->line);
   }
   ```
   - Iterates over each method declared in the class (`for (auto &method : s.methods)`).
   - Checks if the method is a function declaration (`if (!method->is<FunctionDecl>()) continue)`).
   - Prepends `"self"` to the method parameters list to ensure that the instance is always passed as the first argument.
   - Calls `compileFunction` to compile the method body into bytecode chunks (`fnChunk`).
   - Creates a `Closure` object wrapping the function chunk (`closureTpl`).
   - Loads the constant string representation of the closure template onto the stack.
   - Emits an opcode to create a function object from the closure template (`Op::MAKE_FUNCTION`).
   - Binds the method to the class using its name (`Op::BIND_METHOD`).

5. **Define Global or Local Class**:
   ```cpp
   if (current_->scopeDepth == 0)
   {
       emit(Op::DEFINE_GLOBAL, addStr(s.name), line);
   }
   else
   {
       declareLocal(s.name, line);
       emit(Op::DEFINE_LOCAL, static_cast<int>(current_->locals.size()) - 1, line);
   }
   ```
   - Depending on the scope depth (`current_->scopeDepth`):
     - If the scope depth is 0, it means the class is being defined globally. The function emits an opcode to define the global class (`Op::DEFINE_GLOBAL`).
     - Otherwise, it declares the class as a local variable and emits an opcode to define the local class (`Op::DEFINE_LOCAL`). The local index is determined by subtracting 1 from the size of the current locals vector.

## Edge Cases
- **Empty Base Class**: If the class does not have a base class (`s.base.empty()`), the inheritance step is skipped.
- **No Methods**: If the class contains no methods, the loop defining methods will not execute, and only the class creation and definition steps will occur.
- **Method Compilation Errors**: If there are errors during the compilation of any method, they should be handled appropriately by the caller of `compileClassDecl`.

## Interactions with Other Components
- **Constant Pool**: The `addConst` function interacts with the constant pool to store and retrieve constants used in the class declaration.
- **Bytecode Emission