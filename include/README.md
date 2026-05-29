# QuantumLanguage Compiler - Disassembler.h

## Overview

The `include/Disassembler.h` header file plays a crucial role in the QuantumLanguage compiler by providing functionalities to disassemble bytecode into human-readable instructions. This process aids in debugging and understanding the intermediate representation generated during compilation.

## Role in Compiler Pipeline

The disassembler operates as a post-compilation step, converting the binary bytecode back into assembly-like text format. This allows developers to inspect the low-level operations performed by the virtual machine, facilitating easier troubleshooting and verification of the compiled code.

## Key Design Decisions and WHY

### Pretty-Printing Instructions

The `disassembleInstruction` function is designed to pretty-print a single instruction from a bytecode chunk. It takes parameters such as the bytecode chunk (`const Chunk &chunk`) and the index of the instruction within the chunk (`size_t idx`). The function outputs the instruction details to a provided stream (`std::ostream &out`). This design decision ensures that each instruction can be easily examined and understood, enhancing the readability of the disassembled output.

### Dumping Entire Chunks

The `disassembleChunk` function extends the functionality of `disassembleInstruction` by allowing the disassembly of an entire bytecode chunk. It also accepts a `const Chunk &chunk` and outputs the disassembled content to a stream (`std::ostream &out`). By dumping the entire chunk, the disassembler provides a comprehensive view of the program's execution flow, which is invaluable for debugging complex programs.

## Major Classes/Functions Overview

### `disassembleInstruction`

- **Purpose**: Pretty-print a single instruction from a bytecode chunk.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk containing the instruction.
  - `size_t idx`: The index of the instruction within the chunk.
  - `std::ostream &out`: The output stream where the instruction details will be written.
- **Return Value**: The number of bytes consumed by the instruction (always 1 for simplicity).

### `disassembleChunk`

- **Purpose**: Dump an entire bytecode chunk into a human-readable format.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk to be disassembled.
  - `std::ostream &out`: The output stream where the disassembled content will be written.

## Tradeoffs

While the disassembler enhances the debugging capabilities of the QuantumLanguage compiler, it introduces some tradeoffs:

1. **Performance Overhead**: Disassembling bytecode adds overhead to the execution process, potentially slowing down the overall performance of the program.
2. **Complexity**: Implementing a robust disassembler requires careful consideration of various opcode formats and their corresponding meanings, adding complexity to the compiler codebase.
3. **Memory Usage**: Storing the disassembled output temporarily or persistently may consume additional memory resources, especially for large programs.

Despite these tradeoffs, the disassembler remains an essential tool for developers working with the QuantumLanguage compiler, offering valuable insights into the program's execution and helping identify potential issues more effectively.