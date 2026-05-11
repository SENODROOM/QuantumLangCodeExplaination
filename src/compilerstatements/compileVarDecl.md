# `compileVarDecl` Function

## Purpose
The `compileVarDecl` function is responsible for compiling variable declarations in the Quantum Language compiler. It processes both global and local variables, handling their initialization appropriately.

## Parameters/Return Value
- **Parameters**:
  - `s`: A reference to a `VariableDeclaration` structure that contains details about the variable being declared, such as its name, whether it's constant (`isConst`), and an optional initializer (`initializer`).
  
- **Return Value**: 
  - None. The function performs operations directly on the bytecode stream without returning a value.

## Detailed Explanation
### Variable Declaration Processing
The function begins by checking if the variable declaration has an initializer. If it does, the function calls `compileExpr(*s.initializer)` to compile the expression that initializes the variable. If there is no initializer, it emits an `Op::LOAD_NIL` operation to load the nil value onto the stack.

### Scope Depth Check
Next, the function checks the current scope depth using `current_->scopeDepth`. This determines whether the variable is a global or a local variable.

#### Global Variables
If `current_->scopeDepth` is equal to 0, indicating that the function is currently in the global scope, the function proceeds to define the variable. Depending on whether the variable is marked as constant (`s.isConst`), it either defines a constant using `emit(s.isConst ? Op::DEFINE_CONST : Op::DEFINE_GLOBAL, addStr(s.name), line)` or a regular global variable.

#### Local Variables
For local variables, the function first declares the local variable using `declareLocal(s.name, line)`, which updates the local symbol table. Then, it emits an `Op::DEFINE_LOCAL` operation to define the local variable. The operand passed to this operation is the index of the local variable in the current scope's locals list, calculated as `static_cast<int>(current_->locals.size()) - 1`.

## Edge Cases
- **No Initializer**: If the variable declaration does not have an initializer, the function ensures that the nil value is loaded onto the stack before defining the variable.
- **Constant Variable**: When declaring a constant variable, the function uses `Op::DEFINE_CONST` instead of `Op::DEFINE_GLOBAL` to ensure that the value cannot be changed after assignment.

## Interactions with Other Components
- **Symbol Table Management**: The function interacts with the symbol table management component through `declareLocal(s.name, line)`, updating the local symbol table with the new variable.
- **Bytecode Emission**: The function uses the bytecode emission component to generate the appropriate bytecode instructions for defining variables and loading initializers.
- **Scope Tracking**: The function relies on the scope tracking mechanism provided by the compiler to determine whether the variable is global or local.

This comprehensive approach ensures that all aspects of variable declarations are handled correctly, maintaining the integrity and functionality of the compiled code.