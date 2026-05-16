# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is responsible for parsing print statements. This function supports both simple print statements and more complex ones that can include optional keyword arguments like `sep`, `end`, `file`, and `flush`. The primary goal of this function is to correctly interpret and construct an AST (Abstract Syntax Tree) node representing the print statement based on the provided source code.

## Parameters/Return Value

### Parameters:
- None explicitly listed in the given snippet, but implied parameters include:
  - `tokens`: A vector of token objects representing the source code.
  - `pos`: An iterator or index indicating the current position within the `tokens` vector.

### Return Value:
- Returns an `ASTNodePtr` object, which is a pointer to an abstract syntax tree node representing the parsed print statement.

## How It Works

The function begins by checking if the next token is a left parenthesis (`(`). If it is, the function assumes that the print statement includes keyword arguments and proceeds accordingly. If not, it treats the print statement as having only positional arguments.

### Parsing Positional Arguments

If the print statement has positional arguments, the function calls `parseExpr()` repeatedly until it encounters the end of the statement or a comma. Each expression returned by `parseExpr()` is added to the `args` vector.

### Parsing Keyword Arguments

If the print statement includes keyword arguments, the function enters a loop where it checks for identifiers followed by an assignment operator (`=`). Based on the identifier, the function performs different actions:

- **sep**: If the identifier is `sep`, the function checks if the next token is a string literal. If so, it updates the `sep` variable with the string's value. Otherwise, it calls `parseExpr()` to consume and discard any non-literal expressions.
  
- **end**: If the identifier is `end`, the function similarly checks for a string literal. If found, it updates the `end_str` variable with the string's value and sets `newline` to `false` because `end=` overrides the default behavior of adding a newline character at the end of the output. Like `sep`, it discards non-literal expressions if encountered.
  
- **file** and **flush**: For these keywords, the function again calls `parseExpr()` to consume and discard any expressions, as they do not affect the construction of the AST node directly.

### Handling Parentheses

After parsing all arguments, whether positional or keyword, the function expects a right parenthesis (`)`). If the expected token is not found, it throws an error message.

### Edge Cases

- **Missing Right Parenthesis**: If the print statement includes keyword arguments but lacks a closing right parenthesis, the function will throw an error indicating that a `')'` was expected.
- **Non-Literal Expressions**: The function allows for non-literal expressions to be included in the print statement, although it discards them when constructing the AST node. This flexibility might be useful for debugging purposes or to allow for more dynamic control over the print output.
- **Empty Print Statement**: While not explicitly handled in the given snippet, the function should ideally handle cases where there are no arguments in the print statement, resulting in an empty list of arguments in the AST node.

## Interactions With Other Components

The `parsePrintStmt` function interacts closely with the rest of the parser, particularly with the `parseExpr()` function, which is used to parse individual expressions within the print statement. Additionally, it uses utility functions like `consume()`, `expect()`, `match()`, and `skipNewlines()` to manage token consumption, error handling, and whitespace skipping during parsing.

Overall, the `parsePrintStmt` function plays a crucial role in accurately interpreting and representing print statements in the Quantum Language compiler's AST, ensuring that the program's intended output is preserved during compilation.