# ParserCore Component of the Quantum Language Compiler

The `ParserCore` component is a crucial module within the Quantum Language (QL) compiler, responsible for transforming the input source code into an Abstract Syntax Tree (AST). This process ensures that the source code strictly conforms to the language's syntactical rules, facilitating precise execution during subsequent phases of compilation.

## Overview

### Functionality
- **check**: Validates whether the current token matches the expected token type and advances the tokenizer if it does.
- **match**: Consumes the current token if it matches the specified token type, otherwise throws an error.
- **atEnd**: Checks if the end of the input has been reached.
- **skipNewlines**: Skips over any newline characters encountered in the source code.
- **parse**: The main function that initiates parsing the source code and constructs the AST.

### File Structure
- `parsercore.h`: Contains declarations for all public functions and classes used by the `ParserCore`.
- `parsercore.cpp`: Implements the functionality declared in `parsercore.h`.

### Overall Flow
1. **Initialization**: The `ParserCore` initializes itself with the source code and sets up the necessary data structures.
2. **Tokenization**: It uses a tokenizer to break down the source code into individual tokens.
3. **Parsing**: The `parse` function starts the parsing process, calling helper functions like `check`, `match`, and `skipNewlines` as needed to construct the AST.
4. **Error Handling**: If any syntax errors are detected, the parser throws appropriate exceptions or error messages.
5. **Completion**: Once parsing is complete, the `ParserCore` returns the constructed AST, which can be further processed by other components of the compiler.

## Usage
To use the `ParserCore` component, include the header file `parsercore.h` in your project and call the `parse` function with the source code as an argument. The function will return an AST representing the parsed source code.

```cpp
#include "parsercore.h"

int main() {
    std::string sourceCode = "qubit q[0];";
    try {
        ASTNode* ast = ParserCore::parse(sourceCode);
        // Process the AST
    } catch (const std::runtime_error& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    return 0;
}
```

## Contributing
Contributions to the `ParserCore` component are welcome! Please follow the guidelines below:

1. Fork the repository on GitHub.
2. Create a new branch for your feature or bug fix.
3. Make changes and commit them with descriptive commit messages.
4. Push your changes to your forked repository.
5. Open a pull request from your fork to the main repository.

For more detailed information on contributing, please refer to our [CONTRIBUTING.md](../CONTRIBUTING.md) file.

## License
The `ParserCore` component is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.

---

Feel free to update the content based on additional features or improvements made to the `ParserCore` component.