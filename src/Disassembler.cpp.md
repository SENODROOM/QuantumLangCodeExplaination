# Disassembler.cpp

## Role in Compiler Pipeline

`Disassembler.cpp` is a crucial part of the Quantum Language compiler's backend, responsible for converting bytecode back into human-readable assembly-like instructions. This reverse-compilation process facilitates debugging, comprehension, and optimization of the compiled code. By reversing the compilation steps, it allows developers to gain deeper insights into the intermediate representation generated during the compilation phase.

## Key Design Decisions and Why

1. **Bytecode Representation**: The disassembler directly interprets the bytecode instructions, which simplifies the conversion process since there is no need for additional abstraction layers.

2. **Human-Readable Output**: The output mimics assembly language, making it easier for developers to understand the low-level operations performed by the bytecode.

3. **Flexibility with Different Bytecode Formats**: The disassembler is designed to handle various bytecode formats, ensuring compatibility across different versions of the Quantum Language compiler.

4. **Efficient Memory Usage**: By processing bytecode in-place and generating output on-the-fly, the disassembler minimizes memory overhead.

## Major Classes/Functions Overview

### `Disassembler` Class
- **Purpose**: Manages the disassembly process, including parsing bytecode and formatting the output.
- **Key Methods**:
  - `disassemble(const std::vector<uint8_t>& bytecode)`: Main method to start the disassembly process.
  - `printInstruction(std::ostream& os, uint8_t opcode, size_t offset)`: Prints a single instruction in assembly format.

### `opName` Function
- **Purpose**: Maps bytecode operation codes to their corresponding names.
- **Usage**: Used within the disassembly loop to convert each opcode into its readable form.

## Tradeoffs

1. **Performance vs. Readability**: While the disassembler provides high readability, it may not be as performant compared to other tools designed for direct bytecode analysis. However, this trade-off is acceptable given the primary goal of aiding development and debugging.

2. **Complexity**: Handling multiple bytecode formats adds complexity to the disassembler. However, this complexity ensures broader usability and flexibility across different compiler versions.

3. **Memory Overhead**: Directly processing bytecode in-place and generating output on-the-fly minimize memory usage, but could potentially increase CPU load due to increased data handling.

Overall, `Disassembler.cpp` plays a vital role in the Quantum Language compiler pipeline, providing valuable insights into the compiled code through human-readable assembly-like instructions. Its design decisions balance performance, readability, and flexibility, making it an essential tool for developers working with the compiler.