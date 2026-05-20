# QuantumLanguage Compiler - Disassembler.h

## Overview

The `include/Disassembler.h` header file is an essential part of the QuantumLanguage compiler, dedicated to providing functionality for disassembling bytecode into human-readable instructions. This capability is critical for debugging, understanding the compiled code, and verifying its correctness during development.

### Role in Compiler Pipeline

The Disassembler module operates at the end of the compiler pipeline, taking the final bytecode output and converting it back into assembly-like text. This process aids developers in tracing the execution flow, inspecting individual operations, and diagnosing issues related to the generated code.

### Key Design Decisions and Why

#### Pretty-Printing Instructions

The `disassembleInstruction` function is designed to pretty-print a single instruction from the bytecode chunk. It takes three parameters:
- `const Chunk &chunk`: The bytecode chunk containing the instruction.
- `size_t idx`: The index of the instruction within the chunk.
- `std::ostream &out`: The output stream where the disassembled instruction will be written.

This function returns the number of bytes consumed by the instruction, which is always 1 for simplicity in this implementation.

**Why**: Pretty-printing provides a clear and readable format for each instruction, making it easier for developers to understand the sequence of operations without delving into the binary format.

#### Dumping Entire Chunks

The `disassembleChunk` function facilitates the disassembly of an entire bytecode chunk. It accepts two parameters:
- `const Chunk &chunk`: The bytecode chunk to be disassembled.
- `std::ostream &out`: The output stream where the disassembled instructions will be written.

This function iterates through each instruction in the chunk, calling `disassembleInstruction` to print them sequentially.

**Why**: Dumping the entire chunk allows developers to view the complete program's bytecode in a structured manner, aiding in comprehensive analysis and troubleshooting.

### Major Classes/Functions Overview

#### `disassembleInstruction`
- **Purpose**: Pretty-print a single instruction from the bytecode chunk.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk containing the instruction.
  - `size_t idx`: The index of the instruction within the chunk.
  - `std::ostream &out`: The output stream where the disassembled instruction will be written.
- **Return Value**: Number of bytes consumed by the instruction (always 1).

#### `disassembleChunk`
- **Purpose**: Dump an entire bytecode chunk as human-readable instructions.
- **Parameters**:
  - `const Chunk &chunk`: The bytecode chunk to be disassembled.
  - `std::ostream &out`: The output stream where the disassembled instructions will be written.

### Tradeoffs

#### Simplicity vs. Flexibility
- **Simplicity**: The current implementation assumes a fixed instruction size (1 byte), simplifying the disassembly logic.
- **Flexibility**: For more complex architectures or variable-length instructions, additional complexity would be required to handle different sizes accurately.

#### Readability vs. Performance
- **Readability**: Pretty-printing ensures that each instruction is displayed clearly and concisely.
- **Performance**: While readability is prioritized, performance could be impacted slightly due to the overhead of string manipulation and output streaming.

Overall, the `Disassembler.h` header file serves as a vital tool for developers, enabling easy inspection and debugging of the QuantumLanguage compiler's output. Its straightforward design makes it suitable for both educational purposes and practical debugging scenarios.