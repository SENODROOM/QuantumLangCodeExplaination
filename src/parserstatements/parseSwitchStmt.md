# parseSwitchStmt

The `parseSwitchStmt` function is responsible for parsing a `switch` statement in the Quantum Language compiler. This function handles the syntax and structure of a `switch` statement, ensuring that it is correctly parsed into an Abstract Syntax Tree (AST). The `switch` statement allows for multiple conditions to be checked against a single variable or expression.

## What It Does

The primary task of `parseSwitchStmt` is to convert a textual representation of a `switch` statement into its corresponding AST node. This involves:
1. Parsing the `switch` keyword.
2. Consuming the opening parenthesis `(` and parsing the switch expression.
3. Consuming the closing parenthesis `)` and the opening brace `{`.
4. Iterating over each case within the `switch` block until the closing brace `}` is encountered.
5. Creating AST nodes for each case and default block, handling the expressions and statements within them.
6. Generating temporary variables (`__sw_val` and `__sw_hit`) to manage the switch logic.

## Why It Works This Way

This design ensures that the `switch` statement is accurately represented in the AST, allowing for proper semantic analysis and code generation. By using temporary variables, the compiler can track whether a match has been found and handle fall-through behavior correctly.

### Temporary Variables

- **`__sw_val`**: Holds the value of the switch expression.
- **`__sw_hit`**: A boolean flag indicating whether any case has been matched.

These variables help in managing the flow of the `switch` statement, especially when dealing with fall-through cases where subsequent cases should execute even if they don't explicitly have a `break` statement.

## Parameters/Return Value

### Parameters

- None

### Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed `switch` statement.

## Edge Cases

1. **Empty Case Body**: If a case has an empty body, the parser will still create an empty `BlockStmt` for that case.
2. **Fall Through**: The parser handles fall-through behavior by setting the `__sw_hit` variable to `true` once a case is matched. Subsequent cases without explicit `break` statements will continue executing.
3. **Syntax Errors**: If the syntax of the `switch` statement is incorrect (e.g., missing parentheses, braces, or colons), the parser will throw an error with a descriptive message.

## Interactions With Other Components

### Parser Statements Module

- **Consumes Tokens**: The parser consumes tokens as it progresses through the source code, identifying keywords like `switch`, `case`, and `default`.
- **Parses Expressions**: The `parseExpr` function is used to parse the expressions within the `switch` statement and each case.
- **Manages Blocks**: The `skipNewlines` function is called to ensure that the parser skips any unnecessary whitespace between tokens. The `BlockStmt` class is used to represent the body of the `switch` and each case.

### Semantic Analysis

- **Temporary Variable Management**: The `__sw_val` and `__sw_hit` variables are managed during parsing to facilitate correct semantic analysis.
- **Fall Through Handling**: The `__sw_hit` variable helps in handling fall-through scenarios, which are crucial for the proper execution of `switch` statements.

### Code Generation

- **Variable Declarations**: The temporary variables are declared in the AST, which is then used by the code generator to produce the appropriate machine code or intermediate representation.
- **Conditional Logic**: The generated code will use the `__sw_val` and `__sw_hit` variables to implement the conditional logic of the `switch` statement.

In summary, the `parseSwitchStmt` function plays a critical role in parsing `switch` statements, ensuring their accurate representation in the AST. This facilitates further processing such as semantic analysis and code generation, making it essential for the overall functionality of the Quantum Language compiler.