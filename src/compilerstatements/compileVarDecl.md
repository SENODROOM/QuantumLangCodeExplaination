# `compileVarDecl` Function

## Purpose
The `compileVarDecl` function processes variable declarations in the Quantum Language compiler. It ensures that both global and local variables are correctly initialized and defined during the compilation process. This function plays a critical role in maintaining the scope and lifetime of variables throughout the program.

## Parameters/Return Value
- **Parameters**:
  - `s`: A reference to a `VariableDeclaration` structure containing details about the variable being declared, such as its name, whether it's a constant (`isConst`), and an optional initializer expression (`initializer`).
  
- **Return Value**: None. The function directly interacts with the compiler's internal state to define or initialize variables.

## How It Works
The function first checks if the variable has an initializer. If it does, it compiles the initializer expression using `compileExpr(*s.initializer)`. If there is no initializer, it emits an `Op::LOAD_NIL` operation to load the nil value onto the stack, effectively initializing the variable to nil.

Next, based on the current scope depth (`current_->scopeDepth`), the function decides how to handle the variable declaration:
- If the current scope depth is zero (`current_->scopeDepth == 0`), indicating a global scope, the function defines the variable either as a constant (`Op::DEFINE_CONST`) or a regular global variable (`Op::DEFINE_GLOBAL`). It uses the `addStr(s.name)` function to get the string index of the variable's name and emits the appropriate opcode along with the line number where the declaration occurs.
  
- If the current scope depth is greater than zero, indicating a local scope, the function declares the local variable using the `declareLocal(s.name, line)` method. This method updates the compiler's internal state to keep track of local variables. After declaring the local variable, the function emits an `Op::DEFINE_LOCAL` operation. The operand passed to this operation is the index of the local variable within the current scope, calculated as `static_cast<int>(current_->locals.size()) - 1`, and includes the line number where the declaration occurs.

## Edge Cases
- **No Initializer**: When a variable is declared without an initializer, the function initializes it to nil. This prevents uninitialized variables which could lead to undefined behavior at runtime.
  
- **Global vs Local Scope**: The function distinguishes between global and local scopes to ensure that variables are properly defined according to their scope rules. Global variables can be accessed from anywhere in the program, while local variables have limited visibility within their defining block.

## Interactions With Other Components
- **Scope Management**: The function interacts with the scope management component of the compiler through methods like `declareLocal()`. These methods help maintain the correct context for variable declarations and accesses.
  
- **Expression Compilation**: If the variable has an initializer, the function calls `compileExpr()` to compile the initializer expression. This involves evaluating the expression and pushing the result onto the stack, which is then used to initialize the variable.
  
- **String Table**: The function uses `addStr(s.name)` to retrieve the string index of the variable's name. This index is essential for identifying the variable in the compiler's data structures and emitting the correct opcodes.

By handling both initialization and scope definition, the `compileVarDecl` function ensures that variables are correctly managed throughout the compilation process, contributing to the overall correctness and efficiency of the generated code.