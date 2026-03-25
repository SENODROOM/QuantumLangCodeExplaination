# `get` Function

## Overview
The `get` function is a crucial method within the Quantum Language compiler used to retrieve the value of a variable specified by its name (`name`). This function facilitates access to variables within the current lexical scope and seamlessly navigates up the scope hierarchy if the variable is not found in the local scope.

### Parameters
- **`name`**: A string representing the name of the variable whose value needs to be fetched.

### Return Value
- The value associated with the variable named `name`. If the variable exists in either the local scope (`cells`) or any parent scopes, the corresponding value is returned.
- Throws an exception of type `NameError` if the variable is undefined in both the local scope and all parent scopes.

### Edge Cases
1. **Local Variable Existence**: If the variable `name` exists in the local scope (`cells`), the function returns the value directly without searching further.
2. **Parent Scope Search**: If the variable `name` is not found in the local scope, the function recursively searches in each parent scope until the variable is found or the root scope is reached.
3. **Undefined Variable**: If the variable `name` does not exist in either the local scope or any parent scopes, the function throws a `NameError`, indicating that the variable is undefined.

### Interactions with Other Components
- **Scope Management**: The `get` function interacts closely with the scope management system, which maintains two primary maps:
  - `vars`: Stores variables defined at the current scope level.
  - `cells`: Stores variables that have been modified using the reference operator (`&`).
- **Recursive Lookup**: When the variable is not found in the local scope, the function calls itself on the parent scope (`if (parent) return parent->get(name);`). This recursive approach ensures that the entire scope hierarchy is searched for the variable.
- **Exception Handling**: If the variable remains undefined after checking all scopes, the function throws a `NameError`, which is likely handled by higher-level error management mechanisms in the compiler.

### Implementation Details
The implementation of the `get` function is straightforward and involves a series of checks:

1. **Check Local Variables (`cells`)**:
   ```cpp
   auto cit = cells.find(name);
   if (cit != cells.end())
       return *cit->second;
   ```
   - The function first attempts to find the variable `name` in the `cells` map.
   - If found, it dereferences the pointer stored in `cells[name]` and returns the value.

2. **Check Current Scope Variables (`vars`)**:
   ```cpp
   auto it = vars.find(name);
   if (it != vars.end())
       return it->second;
   ```
   - If the variable is not found in `cells`, the function then looks for it in the `vars` map.
   - If found, it simply returns the value stored in `vars[name]`.

3. **Search Parent Scopes**:
   ```cpp
   if (parent)
       return parent->get(name);
   ```
   - If the variable is still not found in the current scope, the function checks if there is a parent scope (`if (parent)`).
   - If a parent scope exists, it recursively calls the `get` function on the parent scope (`parent->get(name)`).

4. **Throw Exception for Undefined Variable**:
   ```cpp
   throw NameError("Undefined variable: '" + name + "'");
   ```
   - If the variable is not found in any scope, including the parent scopes, the function throws a `NameError` with a message indicating that the variable is undefined.

This function is essential for resolving variable references during compilation, ensuring that the correct values are accessed based on the current scope and handling cases where variables may be referenced before they are defined.