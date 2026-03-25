# parseCoutStmt

The `parseCoutStmt` function in the Quantum Language compiler is responsible for parsing statements that involve outputting data to the console using the `cout` keyword. This function constructs an abstract syntax tree (AST) node representing a print statement based on the parsed input.

## What It Does

The primary task of `parseCoutStmt` is to read and parse segments of an output statement, such as `cout << expr1 << expr2 << endl;`. It handles multiple expressions separated by the `<<` operator and optionally includes a newline character (`endl`). The function ensures that the `<<` operator is treated correctly as a stream insertion separator rather than a bitwise shift operator.

## Why It Works This Way

### Handling Bitwise Shifts

In C++, the `<<` operator can be used both for bitwise shifting and as a stream insertion operator. However, when parsing code, it's crucial to distinguish between these uses accurately. The `parseCoutStmt` function avoids calling `parseExpr()` directly because doing so would cause `parseShift()` within it to greedily consume `<<` as a bitwise shift operator. By instead calling `parseAddSub()`, which is one level below `parseShift()`, the function ensures that each `<<` remains available as the stream insertion separator.

### Parsing Expressions

The function iterates through the input tokens, consuming them until it encounters a token that is not `<<`. For each segment following `<<`, it calls `parseAddSub()` to parse the expression at the appropriate precedence level. This prevents `<<` from being incorrectly interpreted as a bitwise shift operator.

### Handling Newlines

If the segment after `<<` is the identifier `endl`, the function sets the `newline` flag to `true` and continues parsing. If the segment is a string literal containing `\n`, the function also sets the `newline` flag to `true`. Otherwise, it adds the expression to the list of arguments.

## Parameters/Return Value

- **Parameters**: None explicitly defined in the provided code snippet.
- **Return Value**: Returns a unique pointer to an ASTNode representing a PrintStmt. The PrintStmt contains a vector of arguments and a boolean indicating whether a newline should be printed.

## Edge Cases

- **Empty Statement**: If there are no expressions or `endl` following `cout`, the function returns a PrintStmt with an empty argument list and `newline` set to `false`.
- **String Literals**: String literals ending with `\n` are treated as special cases where a newline is included without adding the literal to the arguments.
- **Bitwise Shifts**: The function correctly distinguishes between `<<` used as a bitwise shift operator and `<<` used as a stream insertion operator.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token during parsing.
- **Error Handling**: While not shown in the snippet, the function likely interacts with error handling mechanisms to report syntax errors if unexpected tokens are encountered.
- **Precedence Parsing**: The use of `parseAddSub()` demonstrates how the function leverages higher-level parsing functions to handle different levels of operator precedence correctly.
- **Abstract Syntax Tree Construction**: The function contributes to building the AST by creating nodes for print statements and their associated arguments.

This detailed explanation covers the functionality, rationale, and context of the `parseCoutStmt` function within the Quantum Language compiler.