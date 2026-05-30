# `parseImportStmt`

## Purpose
The `parseImportStmt` function is responsible for parsing import statements within the Quantum Language source code. It constructs an `ImportStmt` object that encapsulates the details of the parsed import statement and returns a unique pointer to an `ASTNode`.

## Parameters
- None

## Return Value
- Returns a unique pointer to an `ASTNode`, which contains the parsed import statement.

## How It Works
The function parses two types of import statements:
1. **From Statement**: 
   - Starts with the keyword `from`.
   - Followed by a module name (which can be a dot-separated sequence of identifiers).
   - Ends with the keyword `import`.
   - Then, it expects one or more items to import, each being an identifier optionally followed by an alias using the `as` keyword.

2. **Direct Statement**:
   - Starts with the keyword `import`.
   - Does not specify a base module.
   - Expects one or more items to import, each being an identifier optionally followed by an alias using the `as` keyword.

For both types of statements, the function reads the module or item names and aliases, constructs an `ImportStmt::Item` for each, and adds them to the `imports` vector of the `ImportStmt` object. After parsing all items, it consumes any trailing newline characters or semicolons until it reaches the end of the statement.

## Edge Cases
- If the statement starts with `from` but does not follow with a valid module name or `import`, the function will throw an error.
- If an item to import is specified without an alias, the alias will default to the item's name.
- If there are multiple items to import, they must be separated by commas.
- Trailing newlines or semicolons are ignored and do not affect the parsing of the import statement.

## Interactions with Other Components
- This function interacts with the lexer to retrieve tokens and determine their type.
- It uses helper functions like `expect`, `match`, and `consume` to handle token expectations, matches, and consumption respectively.
- The parsed `ImportStmt` object is then used by the AST builder to construct the corresponding abstract syntax tree node.

Overall, the `parseImportStmt` function ensures that import statements are correctly parsed and represented in the quantum language's abstract syntax tree, facilitating further processing and compilation steps.