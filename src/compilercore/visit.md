# `visit` Function

## Purpose
The `visit` function is a template method within the Quantum Language compiler's `CompilerCore` class. Its primary purpose is to process and translate various types of abstract syntax tree (AST) nodes into corresponding quantum machine code instructions. This function acts as a dispatcher that selects the appropriate compilation routine based on the type of AST node encountered during traversal.

## Parameters
- `node`: A reference to an AST node (`Node& n`) which represents the current node being processed in the AST.
- `ln`: An integer representing the line number where the current node appears in the source code. This parameter is used for error reporting and debugging purposes.

## Return Value
- The return type of the `visit` function is void. It directly modifies the internal state of the `CompilerCore` instance by emitting quantum machine code instructions.

## How It Works
The `visit` function uses C++20's `if constexpr` feature to perform compile-time dispatching based on the type of the AST node. Here’s how it operates:

1. **Type Deduction**: The function deduces the type of the `node` using `std::decay_t<decltype(n)>`. This ensures that any cv-qualifiers or references are removed, leaving just the underlying type.

2. **Conditional Compilation**: Depending on the type of the `node`, the function calls different methods:
    - `BlockStmt`: Calls `compileBlock(n)` to handle block statements.
    - `VarDecl`: Calls `compileVarDecl(n, ln)` to handle variable declarations.
    - `FunctionDecl`: Calls `compileFunctionDecl(n, ln)` to handle function declarations.
    - `ClassDecl`: Calls `compileClassDecl(n, ln)` to handle class declarations.
    - `IfStmt`: Calls `compileIf(n, ln)` to handle conditional statements.
    - `WhileStmt`: Calls `compileWhile(n, ln)` to handle while loops.
    - `ForStmt`: Calls `compileFor(n, ln)` to handle for loops.
    - `ReturnStmt`: Calls `compileReturn(n, ln)` to handle return statements.
    - `PrintStmt`: Calls `compilePrint(n, ln)` to handle print statements.
    - `InputStmt`: Calls `compileInput(n, ln)` to handle input statements.
    - `TryStmt`: Calls `compileTry(n, ln)` to handle try-except blocks.
    - `RaiseStmt`: Calls `compileRaise(n, ln)` to handle raise statements.
    - `BreakStmt`: Emits a break instruction using `emitBreak(ln)`.
    - `ContinueStmt`: Emits a continue instruction using `emitContinue(ln)`.
    - `ImportStmt`: Does nothing since native handling of imports is assumed.
    - `ExprStmt`: Compiles the expression inside the statement using `compileExpr(*n.expr)` and then pops the result off the stack using `emit(Op::POP, 0, ln)`.

3. **Default Case**: If the node type is not explicitly handled, the function falls back to compiling the expression and popping the result off the stack.

## Edge Cases
- **Unknown Node Types**: If the node type is not recognized, the function will still attempt to compile the expression but may produce incorrect or incomplete machine code.
- **Empty Statements**: Statements like `break;` and `continue;` do not have expressions, so they are handled separately without calling `compileExpr`.

## Interactions with Other Components
- **AST Traversal**: The `visit` function is part of a larger AST traversal mechanism. It is typically called recursively for each child node of the current node being processed.
- **Machine Code Emission**: The function interacts closely with the machine code emission logic. It calls methods like `compileBlock`, `compileVarDecl`, etc., which generate specific machine code instructions.
- **Error Handling**: The `ln` parameter is used to report errors at the correct line number in the source code. This helps in debugging and maintaining accurate error messages.

Overall, the `visit` function serves as a crucial component in translating high-level quantum language constructs into low-level quantum machine code, ensuring that each type of AST node is appropriately handled and translated.