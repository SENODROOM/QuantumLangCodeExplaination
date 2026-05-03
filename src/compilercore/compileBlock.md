# `compileBlock`

## Purpose

The `compileBlock` function is responsible for compiling a block of statements in the quantum language. It ensures that all functions declared within the block are properly defined and initialized before any other statements are compiled. This function plays a crucial role in maintaining the correct order of operations during compilation, which is essential for the execution of the generated code.

## Parameters

- `b`: A reference to a `Block` object containing the statements to be compiled.

## Return Value

This function does not return a value explicitly. Instead, it compiles each statement in the provided block, modifying the internal state of the compiler accordingly.

## Edge Cases

1. **Empty Block**: If the block `b` contains no statements, the function will simply exit without performing any operations.
2. **Nested Blocks**: The function checks if the current scope depth (`current_->scopeDepth`) is greater than zero. If so, it processes the statements within the block. This allows the function to handle nested blocks correctly, ensuring that inner block functions are defined before outer block functions.
3. **Function Declarations**: The function iterates over each statement in the block and checks if it is a `FunctionDecl`. If a statement is not a function declaration, it is skipped. This behavior ensures that only function declarations are processed for definition and initialization.
4. **Duplicate Function Names**: If a function name is encountered more than once within the same block, the function will skip redeclaring and defining it again. This prevents errors related to duplicate function definitions.

## Interactions with Other Components

- **Scope Management**: The function interacts with the scope management system through `current_->scopeDepth`, `resolveLocal`, `declareLocal`, and `emit`.
- **Statement Compilation**: The function calls `compileNode` on each statement within the block, allowing other components to handle the actual compilation of individual statements.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function may interact with error handling mechanisms to report issues such as undefined functions or syntax errors.

## Detailed Explanation

### Scope Depth Check

The function starts by checking if the current scope depth (`current_->scopeDepth`) is greater than zero. This condition is crucial because it indicates whether the block is part of an enclosing scope. If the block is nested, the function will process its contents to ensure proper function definition and initialization.

```cpp
if (current_->scopeDepth > 0)
{
    // Process statements within the block
}
```

### Function Declaration Processing

If the block is part of an enclosing scope, the function proceeds to iterate over each statement in the block. For each statement, it checks if the statement is a `FunctionDecl`. If it is not, the function skips to the next statement.

```cpp
for (auto &stmt : b.statements)
{
    if (!stmt->is<FunctionDecl>())
        continue;
    // Process function declarations
}
```

### Duplicate Function Name Handling

Within the loop, the function checks if the local symbol table already contains a function with the same name using `resolveLocal`. If the function name is found in the local symbol table, indicating that it has been previously declared, the function continues to the next statement. Otherwise, it emits a `LOAD_NIL` operation to initialize the function variable and declares the local symbol.

```cpp
auto &fn = stmt->as<FunctionDecl>();
if (resolveLocal(current_, fn.name) != -1)
    continue;
emit(Op::LOAD_NIL, 0, stmt->line);
declareLocal(fn.name, stmt->line);
```

### Define Local Operation

After declaring the local symbol, the function emits a `DEFINE_LOCAL` operation to define the function in the local symbol table. The index used for the `DEFINE_LOCAL` operation is calculated based on the size of the `current_->locals` vector.

```cpp
emit(Op::DEFINE_LOCAL, static_cast<int>(current_->locals.size()) - 1, stmt->line);
```

### Statement Compilation

Once all function declarations have been processed, the function iterates over the remaining statements in the block and calls `compileNode` on each one. This allows other components of the compiler to handle the compilation of individual statements.

```cpp
for (auto &stmt : b.statements)
    compileNode(*stmt);
```

By following this approach, the `compileBlock` function ensures that all function declarations within a block are properly defined and initialized before any other statements are executed. This helps maintain the correct order of operations during compilation and prevents runtime errors related to undefined functions.