# `visit` Function

## Purpose
The `visit` function is a template method within the Quantum Language compiler's `CompilerCore` class. Its primary purpose is to process and translate various types of abstract syntax tree (AST) nodes into corresponding quantum machine code instructions. This function serves as a dispatcher that calls specialized methods based on the type of AST node encountered during compilation.

## Parameters
- `n`: The AST node to be processed. It is a generic parameter that allows the function to handle different types of AST nodes through template specialization.
- `ln`: A line number associated with the current AST node, used for error reporting and debugging purposes.

## Return Value
The return type of the `visit` function is `void`. Each specialized method called within the function also returns `void`, as they perform in-place translation of the AST nodes into quantum machine code instructions.

## How It Works
The `visit` function uses C++20's `if constexpr` feature to determine the type of the AST node at compile time. Depending on the type of the node, it calls one of several specialized methods (`compileIdentifier`, `compileBinary`, etc.). These methods contain the logic necessary to translate the specific AST node into quantum machine code instructions.

For example:
- If the node is a `NumberLiteral`, it emits an `Op::LOAD_CONST` instruction followed by the constant value.
- If the node is a `StringLiteral`, it emits an `Op::LOAD_CONST` instruction followed by the string value.
- If the node is a `BoolLiteral`, it emits either `Op::LOAD_TRUE` or `Op::LOAD_FALSE` depending on the boolean value.

This approach ensures that each AST node is handled efficiently and correctly, leveraging compile-time dispatch to avoid runtime overhead.

## Edge Cases
- **Unknown Node Types**: If the AST node type is not recognized by any of the `if constexpr` conditions, the function will likely result in a compile-time error. This is intentional to catch and fix issues early in the development process.
- **Empty Nodes**: Some AST nodes might be empty or null. Proper handling of these cases should be implemented in the respective specialized methods to ensure robustness.

## Interactions With Other Components
The `visit` function interacts closely with other components of the Quantum Language compiler, including but not limited to:

- **Symbol Table**: Used to resolve identifiers and retrieve their corresponding symbols.
- **Emit Function**: Responsible for generating quantum machine code instructions. The `emit` function is called within each specialized method to insert new instructions into the compiled program.
- **Error Handling**: Utilizes the provided line number (`ln`) to report errors accurately when encountering unsupported or malformed AST nodes.

Overall, the `visit` function plays a crucial role in the translation phase of the Quantum Language compiler, ensuring that each AST node is appropriately translated into quantum machine code instructions.