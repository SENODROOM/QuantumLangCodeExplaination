# Disassembler.cpp

## Role in Compiler Pipeline

`Disassembler.cpp` is an essential component of the Quantum Language compiler's backend that converts bytecode back into human-readable assembly-like instructions. This disassembly process aids in debugging, comprehension, and optimization of the compiled code. By reversing the compilation steps, it enables developers to gain deeper insights into the intermediate representation of their programs.

## Key Design Decisions and Why

1. **Opcode Mapping**: The `opName` function maps each opcode to its corresponding string name. This mapping is crucial for generating readable assembly instructions, making it easier for developers to understand the sequence of operations executed by the bytecode.

2. **Human-Readable Format**: The disassembled output is formatted in a way that resembles assembly language. Each instruction includes the operation name, operands, and line numbers, providing a clear and structured view of the bytecode.

3. **Debugging Support**: The disassembler includes detailed information such as line numbers and operand values, which are invaluable for debugging purposes. This level of detail helps developers pinpoint issues more accurately and efficiently.

4. **Optimization Opportunities**: By understanding the assembly-like representation of the bytecode, developers can identify potential areas for optimization. This insight allows them to make informed decisions about how to improve the performance of their programs.

## Major Classes/Functions Overview

### `class Disassembler`
- **Purpose**: Manages the disassembly process, converting bytecode into human-readable assembly instructions.
- **Methods**:
  - `disassemble(const std::vector<uint8_t>& bytecode)`: Main method that takes bytecode as input and returns the disassembled assembly instructions.
  - `printInstruction(std::ostream& os, uint8_t opCode, size_t pc)`: Helper method to print individual assembly instructions.

### `static const char* opName(Op op)`
- **Purpose**: Provides a human-readable name for each opcode.
- **Functionality**: Uses a switch statement to map each opcode to its corresponding string name.

## Tradeoffs

1. **Performance vs. Readability**: While the disassembler enhances readability and debugging capabilities, it may introduce some overhead during the disassembly process. Balancing these factors is essential for maintaining efficient performance while still providing valuable insights.

2. **Complexity**: Implementing a comprehensive disassembler requires handling various opcodes and ensuring accurate mappings. This complexity adds to the development time but improves the overall functionality of the compiler.

3. **Memory Usage**: Storing the disassembled output in memory consumes additional resources. Optimizing memory usage while maintaining readability is a challenge that needs to be addressed.

By carefully considering these tradeoffs and implementing robust design decisions, the `Disassembler.cpp` file contributes significantly to the effectiveness and usability of the Quantum Language compiler.