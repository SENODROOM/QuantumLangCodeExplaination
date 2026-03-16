# `visit` Function

## Purpose
The `visit` function is a template method in the Quantum Language compiler that processes and compiles different types of nodes within the abstract syntax tree (AST). Each node type corresponds to a specific language construct such as blocks, variables, functions, classes, control structures like if-else, loops, returns, prints, inputs, try-except, raises, breaks, continues, and expressions. The function ensures that each node is handled appropriately based on its type.

## Parameters
- `node`: A reference to the current AST node being processed.
- `ln`: An integer representing the line number where the node appears in the source code. This parameter is used for error reporting and debugging purposes.

## Return Value
The `visit` function does not return any value explicitly. Instead, it performs actions such as compiling statements, emitting operations, and handling special constructs like imports.

## Edge Cases
1. **Import Statements**: When an import statement is encountered, the function simply ignores it because native modules or libraries handle these.
2. **Expression Statements**: If a node is an expression statement (`ExprStmt`), the function compiles the expression and then pops the result off the stack using the `emit(Op::POP, 0, ln)` operation. This ensures that the expression's result is not left on the stack after execution.
3. **Unknown Node Types**: If the node type is not recognized, the function falls back to treating it as an expression statement and emits the same pop operation.

## Interaction with Other Components
- **Compilation Logic**: The `visit` function interacts with various compilation logic methods such as `compileBlock`, `compileVarDecl`, `compileFunctionDecl`, etc., depending on the node type. These methods generate machine code or intermediate representation (IR) for their respective constructs.
- **Error Handling**: The line number (`ln`) passed to the `visit` function helps in identifying the exact location of errors during compilation, which aids in debugging.
- **Stack Management**: Operations like `emit(Op::POP, 0, ln)` manage the stack, ensuring that temporary results from expressions are cleared after use, preventing memory leaks or incorrect program behavior due to residual values on the stack.

## Implementation Details
The implementation uses C++'s `if constexpr` feature to perform compile-time dispatch based on the type of the AST node. This approach avoids runtime overhead associated with virtual function calls and makes the code more efficient and easier to understand.

Here's a breakdown of how the function handles different node types:

- **Blocks (`BlockStmt`)**: Calls `compileBlock(n)` to process the block of statements.
- **Variable Declarations (`VarDecl`)**: Calls `compileVarDecl(n, ln)` to handle variable declarations.
- **Function Declarations (`FunctionDecl`)**: Calls `compileFunctionDecl(n, ln)` to compile function definitions.
- **Class Declarations (`ClassDecl`)**: Calls `compileClassDecl(n, ln)` to process class definitions.
- **Conditional Statements (`IfStmt`)**: Calls `compileIf(n, ln)` to handle conditional logic.
- **Loop Statements (`WhileStmt`, `ForStmt`)**: Calls `compileWhile(n, ln)` and `compileFor(n, ln)` respectively to compile loop constructs.
- **Return Statements (`ReturnStmt`)**: Calls `compileReturn(n, ln)` to handle return statements.
- **Print Statements (`PrintStmt`)**: Calls `compilePrint(n, ln)` to compile print commands.
- **Input Statements (`InputStmt`)**: Calls `compileInput(n, ln)` to handle input operations.
- **Exception Handling (`TryStmt`, `RaiseStmt`)**: Calls `compileTry(n, ln)` and `compileRaise(n, ln)` to process try-except blocks and raise exceptions.
- **Control Flow (`BreakStmt`, `ContinueStmt`)**: Emits `Op::BREAK` and `Op::CONTINUE` operations directly to handle break and continue statements.
- **Imports (`ImportStmt`)**: No action is taken; imports are handled by native components.
- **Expressions (`ExprStmt`)**: Compiles the expression and then pops the result from the stack.

This structure allows the compiler to efficiently handle different types of AST nodes while maintaining readability and performance.