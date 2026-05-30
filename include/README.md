# QuantumLanguage Compiler - Disassembler.h

## Overview

The `include/Disassembler.h` header file is integral to the QuantumLanguage compiler, focusing on the disassembly functionality. It provides utilities to convert bytecode into human-readable assembly-like instructions, aiding debugging and understanding of the compiled code.

## Role in Compiler Pipeline

The Disassembler component operates during the compilation process, specifically after the bytecode generation phase. Its primary role is to facilitate the examination of the compiled code by translating it back into a more readable format. This helps developers identify issues, optimize performance, and gain insights into how their quantum programs are executed at the low level.

### Key Design Decisions and Why

1. **Opcode Handling**: The `disassembleInstruction` function takes an opcode as input and prints its corresponding mnemonic. This ensures that each instruction is correctly identified and represented in the disassembled output.
   
2. **Bytecode Chunk Management**: The `disassembleChunk` function iterates over a bytecode chunk, calling `disassembleInstruction` for each byte. This approach allows for the systematic disassembly of large bytecode files, maintaining readability and organization.

3. **Stream Output**: Both functions use `std::ostream` for output, making them flexible and suitable for various destinations such as console, log files, or other streams. This choice enhances usability and integration within different parts of the compiler.

4. **Single Byte Instructions**: The implementation assumes that each instruction consumes exactly one byte (`bytes consumed always 1`). This simplifies the disassembly logic and ensures consistency across the compiler's architecture.

## Major Classes/Functions Overview

- **disassembleInstruction(const Chunk &chunk, size_t idx, std::ostream &out)**:
  - **Purpose**: Converts a single bytecode instruction into a human-readable format.
  - **Parameters**:
    - `const Chunk &chunk`: A reference to the bytecode chunk containing the instruction.
    - `size_t idx`: The index of the instruction within the chunk.
    - `std::ostream &out`: The output stream where the disassembled instruction will be written.
  - **Return Value**: The number of bytes consumed by the instruction (always 1 in this case).

- **disassembleChunk(const Chunk &chunk, std::ostream &out)**:
  - **Purpose**: Iterates over all instructions in a bytecode chunk and disassembles them using `disassembleInstruction`.
  - **Parameters**:
    - `const Chunk &chunk`: A reference to the bytecode chunk to be disassembled.
    - `std::ostream &out`: The output stream where the disassembled chunk will be written.
  - **Return Value**: None.

## Tradeoffs

- **Simplicity vs. Flexibility**: By assuming each instruction consumes exactly one byte, the disassembler becomes simpler and easier to implement. However, this might limit its ability to handle complex or variable-length instructions in future extensions of the compiler.

- **Performance vs. Readability**: The current implementation focuses on simplicity and ease of use, which may compromise performance slightly compared to more optimized disassemblers. For instance, handling variable-length opcodes would require additional logic, potentially impacting performance.

- **Resource Usage**: Using `std::ostream` for output minimizes resource usage, as it does not require dynamic memory allocation for string manipulation. This is particularly beneficial in environments with limited resources.

Overall, the `Disassembler.h` file plays a crucial role in the QuantumLanguage compiler by providing a straightforward way to examine and debug compiled bytecode. While some compromises have been made for simplicity and performance, these choices ensure that the disassembler remains useful and maintainable for the intended purposes.