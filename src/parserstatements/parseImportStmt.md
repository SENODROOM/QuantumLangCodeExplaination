# `parseImportStmt`

## Purpose

The `parseImportStmt` function is responsible for parsing import statements within the Quantum Language source code. Its primary goal is to construct an `ImportStmt` object that accurately represents the parsed import statement and then return a unique pointer to an ASTNode containing this `ImportStmt`.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but it likely relies on global or passed state to determine the current token being processed.
  
- **Return Value**:
  - Returns a `unique_ptr<ASTNode>` where the `ASTNode` contains an `ImportStmt` object representing the parsed import statement, along with the line number of the statement.

## How It Works

The function parses both standard `import` statements and `from ... import` statements according to the Quantum Language syntax rules. Here's how it handles each type:

1. **Standard Import Statement (`import A as B, C`)**:
   - Initializes an empty string for `module`, indicating that the items are being imported directly rather than from a sub-module.
   - Iterates over each item to be imported using a loop.
     - For each item, it expects an identifier which represents the name of the item to import.
     - Optionally checks for an `as` keyword followed by another identifier to specify an alias for the imported item.
     - Adds each item to the `imports` vector of the `ImportStmt` object.

2. **From Module Import Statement (`from module.sub import A, B`)**:
   - Reads an identifier which represents the module name after the `from` keyword.
   - Expects the `import` keyword following the module name.
   - Iterates over each item to be imported using a loop, similar to the standard import case.
     - Each item can optionally have an alias specified after the `as` keyword.
     - Adds each item to the `imports` vector of the `ImportStmt` object.

After parsing the import statement, the function consumes any trailing newline characters or semicolons until it reaches the end of the statement.

## Edge Cases

- **Empty Import List**: If there are no items to import, the function will still create an `ImportStmt` object with an empty `imports` list.
- **Missing Aliases**: The function allows for items to be imported without specifying aliases.
- **Invalid Syntax**: If the syntax of the import statement is incorrect (e.g., missing keywords, unexpected tokens), the function will throw exceptions with appropriate error messages.

## Interactions With Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve the current token and advance through the tokens as it parses the import statement.
- **Error Handling**: The function includes error handling mechanisms to ensure that the syntax of the import statement is correct. This involves checking for expected tokens and throwing exceptions when these expectations are not met.
- **Abstract Syntax Tree (AST)**: After parsing the import statement, the function constructs an `ImportStmt` object and wraps it in an `ASTNode`. This `ASTNode` is then returned, potentially to be used by higher-level components of the compiler for further processing or analysis.

Overall, the `parseImportStmt` function plays a crucial role in interpreting import declarations in the Quantum Language, ensuring that the necessary components are correctly identified and represented in the abstract syntax tree.