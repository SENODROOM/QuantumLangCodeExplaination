# `compileClassDecl` Function

## Purpose
The `compileClassDecl` function in the Quantum Language compiler is responsible for compiling class declarations into bytecode. This process involves several key steps:

1. **Creating the Class Object**: It loads the class name as a constant and creates a new class object using the `Op::MAKE_CLASS` opcode.
2. **Handling Class Inheritance**: If the class has a base class specified, it loads the base class and applies inheritance using the `Op::INHERIT` opcode.
3. **Defining Methods Within the Class**: It iterates through the members of the class (fields and methods) and binds them to the class object. For fields, it initializes them and binds them as methods. For nested classes, it recursively compiles the nested class and binds its methods to the current class.

## Parameters
- `s`: A reference to an `ASTNode` representing the class declaration.
- `line`: An integer representing the line number where the class declaration occurs.

## Return Value
This function does not explicitly return a value. Instead, it emits bytecode instructions that define the class and its members.

## Edge Cases
- **Empty Base Class**: If the class does not have a base class specified (`s.base.empty()`), the `Op::INHERIT` opcode will not be emitted.
- **Nested Classes Without Methods**: If a nested class does not contain any methods, the recursive call to `compileClassDecl` will still occur but will not emit any additional bytecode for those nested classes.
- **Fields Without Initializers**: If a field does not have an initializer, `Op::LOAD_NIL` will be emitted to initialize it.

## Interactions with Other Components
- **Constant Pool Management**: The function uses `addConst` and `addStr` functions to manage constants and strings in the constant pool, ensuring efficient storage and retrieval of class names and values.
- **Bytecode Emission**: It interacts with the bytecode emission system through the `emit` function, which inserts opcodes into the bytecode stream.
- **Recursive Compilation**: When encountering nested classes, `compileClassDecl` calls itself recursively to handle the nested class's compilation.

## Detailed Explanation
### Step-by-Step Breakdown

1. **Create Class Object**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(s.name)), line);
   emit(Op::MAKE_CLASS, 0, line);
   ```
   - `Op::LOAD_CONST` loads the class name as a constant onto the stack.
   - `Op::MAKE_CLASS` creates a new class object based on the loaded constant.

2. **Handle Class Inheritance**:
   ```cpp
   if (!s.base.empty())
   {
       emitLoad(s.base, line);
       emit(Op::INHERIT, 0, line);
   }
   ```
   - If the class has a base class (`!s.base.empty()`), it loads the base class using `emitLoad`.
   - `Op::INHERIT` then applies inheritance to the newly created class object.

3. **Define Methods Within the Class**:
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
   - `bindClassField` is a lambda function that processes each member of the class.
   - If the member is another class declaration (`ClassDecl`), it recursively compiles the nested class and binds its methods to the current class.
   - If the member is a variable declaration (`VarDecl`), it compiles the initializer or assigns `nil` if there is no initializer, and then binds the variable as a method.
   - If the member is an expression statement containing an assignment expression (`AssignExpr`), it compiles the right-hand side of the assignment and binds the left-hand side identifier as a method.

4. **Iterate Through Fields and Methods**:
   ```cpp
   for (auto &field : s.fields)
       bindClassField(field);

   for (auto &method : s.methods)
   {
       if (bindClassField(method))
           continue;
       if (!method->is<FunctionDecl>())
           continue;
       auto &fd = method->as<FunctionDecl>();
   ```
   - The function iterates through all fields and methods defined in the class declaration.
   - For each field or method, it calls `bindClassField`. If `bindClassField` returns `true`, it means the member was successfully bound, and the loop continues to the next member.
   - If `bindClassField` returns `