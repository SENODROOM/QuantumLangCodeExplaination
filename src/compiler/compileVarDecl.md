# `compileVarDecl`

## Purpose

The `compileVarDecl` function is responsible for compiling variable declarations in the Quantum Language compiler. It handles both global and local variable definitions, including their initializers and whether they are constants or not.

## Parameters/Return Value

- **Parameters**:
  - `s`: A reference to a `VariableDeclaration` struct that contains information about the variable being declared, such as its name, type, initializer, and whether it is a constant (`isConst`).

- **Return Value**: None. The function directly modifies the bytecode stream through calls to `emit`.

## How It Works

1. **Initializer Check**:
   - If the variable declaration has an initializer (`s.initializer` is not null), the function compiles the expression of the initializer using `compileExpr(*s.initializer)`.
   - If there is no initializer, it emits a `LOAD_NIL` operation to load the nil value onto the stack.

2. **Scope Depth Check**:
   - The function checks the current scope depth (`current_->scopeDepth`). This determines whether the variable is being declared globally or locally.
   
3. **Global Declaration**:
   - If `current_->scopeDepth` is 0, indicating that the variable is at the global scope:
     - If the variable is marked as a constant (`s.isConst`), it emits a `DEFINE_CONST` operation followed by the variable's name.
     - Otherwise, it emits a `DEFINE_GLOBAL` operation followed by the variable's name.
   
4. **Local Declaration**:
   - If `current_->scopeDepth` is greater than 0, indicating that the variable is within a local scope:
     - The function declares the local variable using `declareLocal(s.name, line)`, which updates the symbol table with the variable's name and position.
     - It then emits a `DEFINE_LOCAL` operation. The operand passed to this operation is the index of the local variable in the current scope's locals array. This index is calculated as `static_cast<int>(current_->locals.size()) - 1`.

## Edge Cases

- **Empty Initializer**: When a variable is declared without an initializer, the function correctly loads the nil value onto the stack.
- **Constant Variable**: Declaring a constant variable (`s.isConst = true`) results in a `DEFINE_CONST` operation instead of `DEFINE_GLOBAL`, ensuring that the variable cannot be reassigned after its initial definition.
- **Nested Scopes**: Variables declared inside nested scopes are handled correctly by updating the symbol table and emitting the appropriate `DEFINE_LOCAL` operations based on the current scope depth.

## Interactions with Other Components

- **Symbol Table**: The function interacts with the symbol table to manage global and local variables. It uses `declareLocal` to update the symbol table when declaring a local variable.
- **Bytecode Emission**: The function relies on the `emit` function to generate bytecode instructions. Depending on whether the variable is global or local, it emits either `DEFINE_GLOBAL` or `DEFINE_LOCAL` operations.
- **Expression Compilation**: If the variable has an initializer, the function also interacts with the expression compilation component through `compileExpr` to handle the initialization expression.

This function is crucial for accurately translating variable declarations into executable bytecode, ensuring proper scoping and handling of constants.