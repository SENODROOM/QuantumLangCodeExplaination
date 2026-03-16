# `evalArrow`

The `evalArrow` function is responsible for evaluating expressions involving the arrow (`->`) operator in the Quantum Language interpreter. This operator is utilized to access members of objects or dictionaries using pointers. The function ensures that the operation is performed safely and correctly, throwing appropriate errors when necessary.

## Function Overview

### Purpose
- To handle the arrow (`->`) operator, which accesses members of objects or dictionaries through a pointer.
- To evaluate the expression and return the value of the specified member.

### Parameters
- `e`: An expression object containing the details of the arrow operation, including the pointer and the member name to be accessed.

### Return Value
- Returns a `QuantumValue` representing the value of the specified member.
- If the member is not found, returns an empty `QuantumValue`.

### Edge Cases
1. **Non-pointer Dereference**: If the expression being evaluated does not result in a pointer, a `RuntimeError` is thrown indicating that the arrow operator requires a pointer.
2. **Null Pointer Dereference**: If the pointer obtained from the expression evaluation is null, a `RuntimeError` is thrown to prevent dereferencing a null pointer.
3. **Member Not Found**: If the specified member is not found in the instance or dictionary, a `TypeError` is thrown.

### Interactions with Other Components
- **Evaluator**: The `evaluate` function is called to get the value of the expression before accessing the member. This interaction ensures that the expression is properly evaluated before attempting any operations.
- **Type Checking**: The function performs type checking to ensure that the expression results in a valid pointer type. If the type check fails, a `RuntimeError` is thrown.
- **Dictionary Access**: If the cell is a dictionary, the `find` method is used to locate the member. If the member is found, its corresponding value is returned; otherwise, an empty `QuantumValue` is returned.
- **Instance Access**: If the cell is an instance, the `getField` method is invoked to retrieve the value of the specified member. If the field is not found, a `TypeError` is thrown.

## Detailed Explanation

### Step-by-step Execution

1. **Evaluate the Expression**:
   ```cpp
   auto ptrVal = evaluate(*e.object);
   ```
   - The `evaluate` function processes the expression stored in `e.object` and returns its value as a `QuantumValue`.
   
2. **Check if the Result is a Pointer**:
   ```cpp
   if (!ptrVal.isPointer())
       throw RuntimeError("Arrow operator requires a pointer (type: " + ptrVal.typeName() + ")");
   ```
   - The function checks if the evaluated value is a pointer using the `isPointer()` method.
   - If it is not a pointer, a `RuntimeError` is thrown, providing information about the incorrect type.

3. **Dereference the Pointer**:
   ```cpp
   auto ptr = ptrVal.asPointer();
   ```
   - The `asPointer()` method converts the `QuantumValue` into a pointer.
   
4. **Check for Null Pointer**:
   ```cpp
   if (ptr->isNull())
       throw RuntimeError("Null pointer dereference via ->");
   ```
   - The function checks if the pointer is null using the `isNull()` method.
   - If the pointer is null, a `RuntimeError` is thrown to prevent undefined behavior.

5. **Access the Member**:
   ```cpp
   auto &cell = *ptr->cell;
   ```
   - The function dereferences the pointer to access the underlying `Cell` object.

6. **Handle Instance Type**:
   ```cpp
   if (cell.isInstance())
   {
       auto inst = cell.asInstance();
       try
       {
           return inst->getField(e.member);
       }
       catch (...)
       {
           throw TypeError("No member '" + e.member + "' on pointed-to instance");
       }
   }
   ```
   - If the `Cell` object is an instance, the `getField` method is called on the instance to retrieve the value of the specified member.
   - If the field retrieval throws an exception, a `TypeError` is caught and rethrown, indicating that the member was not found.

7. **Handle Dictionary Type**:
   ```cpp
   if (cell.isDict())
   {
       auto dict = cell.asDict();
       auto it = dict->find(e.member);
       return it != dict->end() ? it->second : QuantumValue();
   }
   ```
   - If the `Cell` object is a dictionary, the `find` method is used to search for the specified member.
   - If the member is found, its corresponding value is returned; otherwise, an empty `QuantumValue` is returned.

8. **Throw Error for Unsupported Types**:
   ```cpp
   throw RuntimeError("Cannot use -> on type " + cell.typeName());
   ```
   - If the `Cell` object is neither an instance nor a dictionary, a `RuntimeError` is thrown, indicating that the arrow operator cannot be used on the given type.

### Summary
The `evalArrow` function provides a robust mechanism for handling the arrow (`->`) operator in the Quantum Language interpreter. It ensures that the operation is performed only on valid pointers and handles different types of cells (instances and dictionaries) appropriately. By performing thorough type checking and error handling, the function maintains the integrity and safety of the program during execution.