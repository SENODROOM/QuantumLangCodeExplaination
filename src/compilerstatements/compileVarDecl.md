# `compileVarDecl` Function

## Purpose
The `compileVarDecl` function is responsible for compiling variable declarations in the Quantum Language compiler. It handles both global and local variable declarations, as well as their initializers.

## Parameters/Return Value
- **Parameters**: 
  - `s`: A reference to a `VariableDeclaration` object containing information about the variable being declared, such as its name, whether it's a constant (`isConst`), and an optional initializer expression (`initializer`).
  - `line`: An integer representing the current line number in the source code, used for error reporting and debugging purposes.
  
- **Return Value**: None. The function directly modifies the compiled bytecode through calls to `emit`.

## How It Works
1. **Initializer Check**:
   - If the variable declaration includes an initializer (`s.initializer` is not null), the function compiles the initializer using `compileExpr(*s.initializer)`.
   - If there is no initializer, the function emits a `LOAD_NIL` operation to load the nil value onto the stack.

2. **Scope Depth Check**:
   - The function checks the current scope depth (`current_->scopeDepth`). This determines whether the variable is being declared globally or locally.
   
3. **Global Declaration**:
   - If the current scope depth is 0 (`current_->scopeDepth == 0`), indicating that the variable is at the global level, the function proceeds to define the variable.
   - If the variable is marked as a constant (`s.isConst`), it emits a `DEFINE_CONST` operation followed by the variable name (converted to a string using `addStr(s.name)`). This operation defines a constant in the global environment.
   - If the variable is not a constant, it emits a `DEFINE_GLOBAL` operation with the same parameters. This operation defines a non-constant variable in the global environment.

4. **Local Declaration**:
   - If the current scope depth is greater than 0, indicating that the variable is within a local scope, the function declares the local variable first using `declareLocal(s.name, line)`. This step may involve setting up metadata or tracking local variables.
   - After declaring the local variable, the function emits a `DEFINE_LOCAL` operation. The index of the local variable is determined by subtracting 1 from the size of the `locals` vector (`static_cast<int>(current_->locals.size()) - 1`). This operation defines the local variable within the current scope.

## Edge Cases
- **Empty Initializer**: When a variable is declared without an initializer, the function correctly loads the nil value onto the stack.
- **Constant Variable**: Declaring a constant variable results in different bytecode operations compared to a non-constant variable, ensuring proper handling of immutable values.
- **Nested Scopes**: The function correctly distinguishes between global and local variable declarations based on the scope depth, allowing for nested scoping rules in the language.

## Interactions with Other Components
- **Emit Function**: The `emit` function is crucial here as it adds operations to the compiled bytecode. It interacts with the `Op` enum, which likely contains various operation codes like `DEFINE_CONST`, `DEFINE_GLOBAL`, and `LOAD_NIL`.
- **Current Scope**: The function relies on the `current_` pointer, which points to the current compilation context or scope. This pointer is essential for determining whether the variable is global or local.
- **Locals Vector**: In local scope declarations, the function uses the `locals` vector to track the number of local variables and their indices. This interaction ensures that each local variable has a unique identifier within its scope.

Overall, the `compileVarDecl` function plays a vital role in the compilation process by handling variable declarations according to their scope and initialization status, thereby generating appropriate bytecode for the Quantum Language interpreter.