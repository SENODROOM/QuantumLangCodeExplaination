# `compileClassDecl` Function

## Purpose
The `compileClassDecl` function in the Quantum Language compiler is responsible for compiling class declarations into bytecode. This process involves several key steps:
- Creating the class object.
- Handling class inheritance.
- Defining methods within the class.

By following these steps, the function ensures that the class structure is correctly represented in the bytecode, allowing for efficient execution of class-based operations.

## Parameters/Return Value
### Parameters
- `s`: An ASTNode representing the class declaration to be compiled.
- `line`: The line number in the source code where the class declaration occurs.

### Return Value
This function does not explicitly return a value. Instead, it modifies the global state of the compiler by emitting bytecode instructions that define the class and its members.

## Detailed Explanation
### Step-by-Step Breakdown

1. **Loading Class Name**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(s.name)), line);
   ```
   - This instruction loads the name of the class into the bytecode stack. The `addConst` function adds the class name to the constant pool, ensuring uniqueness and efficient storage.

2. **Creating the Class Object**:
   ```cpp
   emit(Op::MAKE_CLASS, 0, line);
   ```
   - The `Op::MAKE_CLASS` opcode creates a new class object on the bytecode stack using the class name loaded previously.

3. **Handling Inheritance**:
   ```cpp
   if (!s.base.empty())
   {
       emitLoad(s.base, line);
       emit(Op::INHERIT, 0, line);
   }
   ```
   - If the class has a base class specified (`!s.base.empty()`), the function emits an instruction to load the base class's name and then uses the `Op::INHERIT` opcode to set up inheritance. This means the newly created class will inherit all properties and methods from its base class.

4. **Binding Class Fields**:
   ```cpp
   auto bindClassField = [&](ASTNodePtr &member)
   {
       if (member->is<ClassDecl>())
       {
           auto &nested = member->as<ClassDecl>();
           compileClassDecl(nested, member->line);
           emitLoad(nested.name, member->line);
           emit(Op::BIND_METHOD, addStr(nested.name), member->line);
           return true;
       }
       if (member->is<VarDecl>())
       {
           auto &field = member->as<VarDecl>();
           if (field.initializer)
               compileExpr(*field.initializer);
           else
               emit(Op::LOAD_NIL, 0, member->line);
           emit(Op::BIND_METHOD, addStr(field.name), member->line);
           return true;
       }
       if (member->is<ExprStmt>() &&
           member->as<ExprStmt>().expr &&
           member->as<ExprStmt>().expr->is<AssignExpr>())
       {
           auto &assign = member->as<ExprStmt>().expr->as<AssignExpr>();
           if (assign.target->is<Identifier>())
           {
               compileExpr(*assign.value);
               emit(Op::BIND_METHOD, addStr(assign.target->as<Identifier>().name), member->line);
               return true;
           }
       }
       return false;
   };
   ```
   - A lambda function `bindClassField` is defined to handle different types of members within the class (fields or nested classes).
   - For each member, it checks whether the member is another class declaration (`ClassDecl`). If so, it recursively compiles the nested class and binds its methods to the current class.
   - If the member is a variable declaration (`VarDecl`), it compiles the initializer expression (if provided) or pushes `nil` onto the stack if there is no initializer. Then, it binds the field as a method.
   - If the member is an expression statement containing an assignment expression (`AssignExpr`) targeting an identifier, it compiles the right-hand side of the assignment and binds it as a method.

5. **Compiling Class Members**:
   ```cpp
   for (auto &field : s.fields)
       bindClassField(field);
   ```
   - Iterates over each field declared within the class and applies the `bindClassField` lambda to compile and bind them as methods.

6. **Compiling Methods**:
   ```cpp
   for (auto &method : s.methods)
   {
       if (bindClassField(method))
           continue;
       if (!method->is<FunctionDecl>())
           continue;
       auto &fd = method->as<FunctionDecl>();

       // Prepend "self" as slot 0 so the instance is always at the first local.
       // The VM calls methods with the instance as the first argument (argCount+1).
       // "this" references are resolved to "self" by emitLoad/emitStore.
       std::vector<std::string> methodParams;
   ```
   - Iterates over each method declared within the class.
   - It first attempts to bind the method using `bindClassField`. If successful, it skips further processing.
   - If the member is not a function declaration (`FunctionDecl`), it continues to the next iteration.
   - If the member is a function declaration, it prepares to compile the method body. The comment explains that the virtual machine (VM) expects the instance (`self`) to be passed as the first argument when calling methods, which is handled internally by the `emitLoad` and `emitStore` functions.

### Edge Cases
- **Empty Base Class**: If the class does not have a base class, the function simply creates the class without any inheritance setup.
- **Nested Classes**: Nested classes are treated as fields and are recursively compiled to ensure