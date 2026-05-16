# resolveUpvalue

## Purpose

The `resolveUpvalue` function plays a crucial role in managing upvalues within nested functions in the Quantum Language compiler. An upvalue is a variable that is accessible in an inner function but defined in an outer function. The primary purpose of this function is to determine whether a given variable name corresponds to an upvalue and, if so, to capture it appropriately.

## Parameters

- `State* state`: A pointer to the current compilation state, which includes information about the enclosing scope and the list of locals.
- `const std::string& name`: The name of the variable to be resolved as an upvalue.

## Return Value

- `int`: Returns the index of the upvalue if found; otherwise, returns `-1`.

## How It Works

### Step 1: Check Enclosing Scope
The function first checks if there is an enclosing scope (`state->enclosing`). If not, it means the variable is not accessible through any upvalue, so it returns `-1`.

### Step 2: Resolve Local Variable
If there is an enclosing scope, the function attempts to resolve the variable name as a local variable within that scope using the `resolveLocal` method. If successful (`local != -1`), it marks the local variable as captured and adds it as an upvalue to the current scope using the `addUpvalue` method with the parameter `true`. The index of the new upvalue is then returned.

### Step 3: Resolve Upvalue
If the variable name is not found as a local variable in the enclosing scope, the function recursively calls itself on the enclosing scope to check further up the scope chain. If an upvalue is found (`upvalue != -1`), it adds this upvalue to the current scope using the `addUpvalue` method with the parameter `false`, indicating that the upvalue is not a local variable but rather a reference to a variable in an even higher enclosing scope. The index of the new upvalue is returned.

### Step 4: Handle Edge Cases
- **No Enclosing Scope**: If there is no enclosing scope, the function immediately returns `-1`.
- **Variable Not Found**: If the variable is not found either as a local or as an upvalue in the enclosing scope, the function continues checking further up the scope chain until it reaches the global scope or returns `-1` if the variable is not found anywhere.

## Interactions With Other Components

- **Scope Management**: The `resolveUpvalue` function interacts closely with the scope management system, utilizing methods like `resolveLocal` and `addUpvalue` to manage the capturing and resolution of upvalues.
- **Compilation State**: The function operates on the current compilation state, which contains information about the scopes and variables being processed during compilation.
- **Recursive Calls**: By calling itself on the enclosing scope, the function demonstrates its recursive nature, allowing it to traverse multiple levels of nested scopes to find the required upvalue.

In summary, the `resolveUpvalue` function is designed to efficiently manage upvalues in nested functions, ensuring that variables can be accessed correctly across different scopes. Its implementation leverages recursion and interaction with the scope management system to achieve this goal.