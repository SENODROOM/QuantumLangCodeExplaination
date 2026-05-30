# `declareLocal`

## Function Overview

The `declareLocal` function is a crucial part of the Quantum Language compiler's core functionality, responsible for managing local variables within the current scope. This function ensures that local variables are correctly declared and tracked during compilation, preventing variable name clashes and maintaining proper scoping rules.

## Parameters/Return Value

- **Parameters**:
  - `name`: A string representing the name of the local variable being declared.
  
- **Return Value**:
  - The function does not explicitly return any value. Instead, it modifies the internal state of the compiler by adding an entry to the list of local variables (`locals`) in the current scope context (`current_`). Each entry contains the variable name, its depth in the scope hierarchy, and a boolean indicating whether the variable has been initialized.

## How It Works

The `declareLocal` function operates under the assumption that there is always a valid scope context (`current_`) available when it is called. If the scope depth is zero, indicating that the function is being called outside of any defined scope, the function simply returns without making any changes.

Here’s how the function works step-by-step:

1. **Check Scope Depth**: 
   - The function first checks if the current scope depth (`current_->scopeDepth`) is zero. If it is, the function returns immediately because it cannot declare a local variable outside of a valid scope.

2. **Add Local Variable Entry**:
   - If the scope depth is greater than zero, the function proceeds to add a new entry to the `locals` vector of the current scope context.
   - Each entry consists of three elements:
     - `name`: The name of the local variable.
     - `current_->scopeDepth`: The depth of the current scope at which the variable is being declared. This helps in tracking the variable's scope and prevents conflicts between variables in different scopes.
     - `false`: A boolean flag indicating whether the variable has been initialized. Initially, all variables are marked as uninitialized.

3. **Maintain Scope Context**:
   - By updating the `locals` vector, the function ensures that the compiler maintains accurate information about all local variables currently in scope. This information is vital for error checking, code generation, and optimization phases of the compilation process.

## Edge Cases

- **Empty Scope**: When calling `declareLocal` outside of any defined scope (i.e., `current_->scopeDepth` is zero), the function does nothing. This handles situations where local variable declarations might occur accidentally or due to logical errors in the source code.
- **Duplicate Declarations**: Although not directly handled by `declareLocal`, the presence of duplicate entries in the `locals` vector can lead to issues such as redeclaring a variable within the same scope. Error handling mechanisms should be implemented elsewhere to catch and report such cases.

## Interactions with Other Components

- **Scope Management**: `declareLocal` interacts closely with the scope management system of the compiler. It relies on the `current_` pointer to access the current scope context, which includes the `locals` vector where variable declarations are stored.
- **Error Checking**: While `declareLocal` itself does not check for variable name clashes, it provides the necessary data structure for later stages of the compiler to perform such checks. Accurate tracking of local variables helps in identifying and reporting errors related to undeclared or misused variables.
- **Code Generation**: During code generation, the compiler uses the information stored in the `locals` vector to allocate memory for local variables, generate appropriate load/store instructions, and handle variable lifetimes efficiently.

In summary, the `declareLocal` function is a fundamental tool for managing local variables in the Quantum Language compiler. Its design ensures that variables are correctly declared and scoped, facilitating robust error checking and efficient code generation.