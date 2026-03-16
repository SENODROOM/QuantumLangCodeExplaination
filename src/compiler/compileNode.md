# `compileNode`

`compileNode` is a crucial function in the Quantum Language compiler that processes and compiles different types of nodes found within the abstract syntax tree (AST). This function ensures that each node is handled appropriately based on its type, thereby generating the corresponding machine code or bytecode.

## What It Does

The primary role of `compileNode` is to traverse through the AST and convert each node into executable instructions. Depending on the type of the node, it may involve compiling expressions, statements, blocks, classes, functions, and control structures like loops and conditionals.

## Why It Works This Way

This design allows `compileNode` to be highly flexible and extensible. By using `std::visit`, it can dynamically dispatch to the appropriate handler based on the type of the node. This approach eliminates the need for manual type checking and casting, making the code cleaner and easier to maintain. Additionally, it enables easy addition of new node types without modifying the existing logic.

## Parameters/Return Value

- **Parameters**:
  - `node`: A reference to the current node being processed. The type of `node` is determined at runtime using `std::visit`.

- **Return Value**: 
  - None. The function directly modifies the output buffer or performs actions related to compilation.

## Edge Cases

1. **Import Statements**: When an `ImportStmt` node is encountered, the function simply returns because native handling mechanisms manage imports.
2. **Empty Nodes**: If a node is empty or not recognized, the function will still attempt to compile it as an expression and pop the result. However, this behavior might vary depending on the context and how the compiler handles such cases.
3. **Unknown Node Types**: Although the function includes a default case to handle unknown node types, it's generally expected that all valid node types should have explicit handlers defined.

## Interactions With Other Components

- **AST Traversal**: `compileNode` is part of the larger AST traversal mechanism. It is called recursively for each child node of the current node.
  
- **Output Buffer**: During compilation, the function generates intermediate instructions which are stored in an output buffer. This buffer accumulates the compiled code until the entire program has been processed.

- **Error Handling**: The function implicitly interacts with error handling mechanisms. If any errors occur during the compilation of a node (e.g., undefined variables, syntax errors), they would typically be reported back to the user or halt the compilation process.

- **Symbol Table**: The function may interact with a symbol table to resolve variable names and their associated metadata (e.g., type information).

- **Control Flow Management**: Functions like `compileIf`, `compileWhile`, and `compileFor` manage control flow constructs. They ensure that the correct sequence of instructions is emitted to handle these structures properly.

Here’s a breakdown of how some key parts of the function work:

### Using `std::visit`
```cpp
std::visit([&](auto &n)
           {
    using T = std::decay_t<decltype(n)>;
    // Handlers for different node types go here...
}, node.node);
```
`std::visit` is used to apply a lambda function to the variant held by `node.node`. The lambda captures the line number (`ln`) and uses `using T = std::decay_t<decltype(n)>;` to get the decayed type of `n`. This allows the function to use `if constexpr` to check the type of `n` at compile time and call the appropriate handler.

### Handling Different Node Types
Each `if constexpr` block corresponds to a specific node type. For example:
```cpp
else if constexpr (std::is_same_v<T, FunctionDecl>)   compileFunctionDecl(n, ln);
```
When a `FunctionDecl` node is encountered, `compileFunctionDecl` is called to handle the function declaration, including its body.

### Compiling Expressions
For `ExprStmt` nodes, the function compiles the expression and then pops the result from the stack:
```cpp
else if constexpr (std::is_same_v<T, ExprStmt>)
{
    compileExpr(*n.expr);
    emit(Op::POP, 0, ln);
}
```
This ensures that the expression is evaluated but the result is discarded unless explicitly needed.

### Emitting Instructions
Finally, regardless of the node type, the function emits an `Op::POP` instruction to remove any remaining values from the stack:
```cpp
else { compileExpr(node); emit(Op::POP, 0, ln); }
```
This helps in maintaining a clean state between different nodes and prevents potential stack overflows or misuse of values.

Overall, `compileNode` serves as a central hub for processing various AST nodes, ensuring that each one is correctly translated into executable instructions. Its modular design and dynamic dispatch make it robust and adaptable to future changes in the language specification.