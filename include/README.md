# QuantumLanguage Compiler - Disassembler.h

## Overview

The `include/Disassembler.h` header file in the QuantumLanguage compiler is essential for converting bytecode into human-readable assembly language instructions. This process aids in debugging and understanding the internal operations of the compiled program. The Disassembler module is integral to the compiler's pipeline, facilitating the transition from low-level machine code to high-level readable format.

## Role in Compiler Pipeline

The Disassembler operates during the compilation phase, specifically after the bytecode has been generated but before it is executed. Its primary function is to convert each opcode in the bytecode into its corresponding assembly language representation, making it easier for developers to analyze and troubleshoot their programs.

### Key Design Decisions and Why

1. **Single Instruction Pretty-Printing**: The `disassembleInstruction` function is designed to pretty-print a single instruction. This decision simplifies the interface and allows for easy integration with other parts of the compiler, such as debuggers or interpreters.

2. **Whole Chunk Dumping**: The `disassembleChunk` function dumps the entire bytecode chunk, providing a comprehensive view of the program's execution flow. This is particularly useful for large programs or when debugging complex issues.

3. **Stream-Based Output**: Both functions utilize `std::ostream` for output, ensuring flexibility and compatibility with various logging and display mechanisms. This approach also aligns well with modern C++ practices of using streams for I/O operations.

## Major Classes/Functions Overview

### `disassembleInstruction`

- **Purpose**: Pretty-prints a single instruction from the given bytecode chunk.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk containing the instruction.
  - `size_t idx`: The index of the instruction within the chunk.
  - `std::ostream &out`: The output stream where the instruction will be printed.
- **Return Value**: Returns the number of bytes consumed by the instruction (always 1 in this case).

### `disassembleChunk`

- **Purpose**: Dumps the entire bytecode chunk, printing each instruction in a readable format.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk to be dumped.
  - `std::ostream &out`: The output stream where the chunk will be printed.
- **Return Value**: None.

## Tradeoffs

1. **Performance vs. Readability**: While disassembling bytecode can provide valuable insights for debugging, it does introduce overhead compared to direct execution. However, the benefits of readability often outweigh the performance cost, especially during development and testing phases.

2. **Flexibility vs. Complexity**: Using `std::ostream` for output offers great flexibility, allowing the disassembler to integrate seamlessly with different logging systems. On the other hand, this flexibility comes at the cost of increased complexity in the implementation.

3. **Memory Usage**: Storing and processing the entire bytecode chunk requires additional memory. For very large programs, this could potentially impact performance, although modern compilers and machines typically handle this efficiently.

Overall, the `Disassembler.h` header file is a vital component of the QuantumLanguage compiler, enhancing the debugging experience and providing a deeper understanding of the program's structure and behavior.