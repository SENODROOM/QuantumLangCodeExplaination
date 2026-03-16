# `compileNew` Function

The `compileNew` function is a crucial method in the Quantum Language compiler's source file `src/Compiler.cpp`. This function is responsible for compiling the creation of new instances of objects or classes. It ensures that the object's type and its arguments are correctly handled before emitting an opcode to create the instance.

## What It Does

The primary role of `compileNew` is to generate the necessary code to instantiate a new object of a specified class or type. This involves:
1. Loading the type name into the program's memory.
2. Compiling each argument passed to the constructor of the object.
3. Emitting an opcode (`Op::INSTANCE_NEW`) to create the instance, along with the number of arguments provided.

## Why It Works This Way

This approach is chosen because it follows a structured and clear sequence of operations:
- **Loading the Type Name**: The type name needs to be loaded first so that the runtime knows which class or type to instantiate. This step is essential for correct object creation.
- **Compiling Arguments**: Each argument must be compiled individually before they can be used as parameters for the constructor. This ensures that all arguments are properly evaluated and converted to their respective types.
- **Emitting the Opcode**: After loading the type name and compiling all arguments, the `Op::INSTANCE_NEW` opcode is emitted. This opcode tells the runtime to create a new instance of the specified class using the given arguments.

By following this sequence, `compileNew` ensures that the object creation process is both accurate and efficient.

## Parameters/Return Value

### Parameters
- `e`: A reference to an expression node representing the object instantiation. This node contains the type name and the list of arguments to be passed to the constructor.
- `line`: An integer representing the current line number in the source code. This information is used for error reporting and debugging purposes.

### Return Value
- None. The function directly interacts with the compiler's internal state and emits opcodes without returning any value.

## Edge Cases

### Empty Argument List
If the expression node `e` has an empty list of arguments, the function will still load the type name and emit the `Op::INSTANCE_NEW` opcode with zero arguments. This is valid since some constructors might not require any parameters.

### Nested Object Instantiation
When dealing with nested object instantiations (i.e., creating an object within another object), `compileNew` ensures that each level of instantiation is handled correctly. It recursively compiles expressions for nested objects.

### Error Handling
Although not explicitly shown in the snippet, `compileNew` should include error handling mechanisms to ensure that the type name exists and that the arguments match the expected types for the constructor.

## Interactions With Other Components

### Lexer and Parser
The `compileNew` function relies on the lexer and parser to correctly identify and parse the type name and arguments in the source code. These components provide the necessary tokens and syntax structures that `compileNew` uses to understand the context of the object instantiation.

### Emitter
The `emit` function is called within `compileNew` to output opcodes into the intermediate representation (IR) of the program. This IR is then further processed by subsequent stages of the compilation process.

### Symbol Table
While not directly visible in the snippet, `compileNew` likely interacts with the symbol table to resolve the type name and check if the arguments are valid for the specified constructor. This interaction ensures type safety during the compilation process.

In summary, the `compileNew` function is a fundamental part of the Quantum Language compiler, responsible for accurately compiling object instantiation. Its design ensures that the type name and arguments are correctly handled before emitting the appropriate opcode, making it a reliable component in the overall compilation pipeline.