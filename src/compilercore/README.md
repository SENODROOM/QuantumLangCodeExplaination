# CompilerCore

The `CompilerCore` component is a crucial part of the Quantum Language compiler, designed to handle essential aspects of compiling quantum programs. It focuses on managing scopes, resolving variables, and emitting bytecode. This module acts as the central hub for processing quantum nodes and expressions, ensuring they are accurately parsed and transformed into executable code.

## Files and Functions

### compile
- **Description**: The primary function that initiates the compilation process.
- **Relationship**: Calls `compileNode` to start the recursive compilation of the program's abstract syntax tree (AST).

### beginScope
- **Description**: Marks the beginning of a new scope, typically used when entering a block or function.
- **Relationship**: Used before compiling any statements within a new scope.

### endScope
- **Description**: Marks the end of a current scope, releasing resources associated with it.
- **Relationship**: Called after all statements within a scope have been compiled.

### resolveLocal
- **Description**: Resolves a local variable within the current scope.
- **Relationship**: Used during expression compilation to find the correct variable reference.

### addUpvalue
- **Description**: Adds an upvalue to the current closure, which is a variable from an enclosing scope.
- **Relationship**: Relevant during the compilation of closures where variables from outer scopes need to be accessed.

### resolveUpvalue
- **Description**: Resolves an upvalue within the current closure.
- **Relationship**: Used to correctly access variables from enclosing scopes.

### declareLocal
- **Description**: Declares a local variable within the current scope.
- **Relationship**: Used during the declaration phase of the program to set up variable storage.

### emitLoad
- **Description**: Emits bytecode to load a value onto the stack.
- **Relationship**: Used during expression compilation to manage data retrieval.

### emitStore
- **Description**: Emits bytecode to store a value from the stack into a variable.
- **Relationship**: Used during statement compilation to manage data assignment.

### beginLoop
- **Description**: Marks the beginning of a loop structure.
- **Relationship**: Used before compiling loop-related statements.

### emitBreak
- **Description**: Emits bytecode to break out of the current loop.
- **Relationship**: Used during the compilation of loop exit conditions.

### emitContinue
- **Description**: Emits bytecode to continue to the next iteration of the current loop.
- **Relationship**: Used during the compilation of loop control flow.

### endLoop
- **Description**: Marks the end of a loop structure.
- **Relationship**: Used after all loop-related statements have been compiled.

### compileNode
- **Description**: Recursively compiles a node in the AST.
- **Relationship**: Central function called by `compile`, `compileBlock`, and `compileExpr`.

### visit
- **Description**: Visits a specific type of node in the AST and calls the appropriate handler.
- **Relationship**: Used by `compileNode` to dispatch handling based on the node type.

### compileBlock
- **Description**: Compiles a block of statements.
- **Relationship**: Calls `compileNode` for each statement within the block.

### compileExpr
- **Description**: Compiles an expression.
- **Relationship**: Calls `visit` to determine the type of expression and handles it accordingly.

## Overall Flow

1. **Initialization**: The `compile` function is called with the root node of the AST.
2. **Scope Management**:
   - `beginScope` is called when entering a new scope (e.g., block, function).
   - `endScope` is called when exiting a scope.
3. **Variable Resolution**:
   - `resolveLocal` is used to find local variables.
   - `addUpvalue` and `resolveUpvalue` manage upvalues for closures.
4. **Bytecode Emission**:
   - `emitLoad` and `emitStore` handle loading and storing values.
5. **Loop Handling**:
   - `beginLoop` and `endLoop` mark the start and end of loops.
   - `emitBreak` and `emitContinue` manage loop control flow.
6. **Recursive Compilation**:
   - `compileNode` recursively processes each node in the AST.
   - `compileBlock` compiles blocks of statements.
   - `compileExpr` compiles individual expressions using `visit`.

By following this structured approach, `CompilerCore` ensures that the quantum program is compiled efficiently and accurately, maintaining proper scope management and variable resolution throughout the compilation process.