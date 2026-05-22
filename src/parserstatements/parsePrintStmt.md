# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is designed to parse print statements. It supports both simple print statements and more complex ones that can include optional keyword arguments such as `sep`, `end`, `file`, and `flush`. This function ensures that the syntax of the print statement is correctly interpreted and constructs an appropriate Abstract Syntax Tree (AST) node representing the print operation.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet.
  
- **Return Value**:
  - An `ASTNodePtr` object representing the parsed print statement. The AST node contains information about the expressions to be printed, along with any keyword arguments specified (`sep`, `end`, `file`, `flush`).

## How It Works

### Parsing Simple Print Statements

If the parser encounters a print statement without parentheses, it assumes a simple print statement and parses the expression immediately following the `print` keyword. For example:

```ql
print Hello, World!
```

In this case, the function will call `parseExpr()` once to get the expression `Hello, World!` and then construct an AST node with this single argument.

### Parsing Complex Print Statements

For complex print statements with parentheses, the function enters a loop where it continues parsing expressions until it encounters a closing parenthesis or reaches the end of the input. Inside the loop, it checks for keyword arguments using the syntax `keyword=value`. If a keyword argument is detected, it consumes the keyword and the equals sign, then processes the value associated with the keyword.

#### Supported Keyword Arguments

- **sep**: Specifies the separator between items to be printed. If not specified, a space character (" ") is used by default.
- **end**: Specifies the string appended at the end of the output. If not specified, a newline character ("\n") is used by default.
- **file**: Specifies the file object to which the output should be written. If not specified, the standard output stream is used.
- **flush**: A boolean indicating whether the output buffer should be flushed after printing. If not specified, the buffer may not be flushed automatically.

#### Example

Consider the following complex print statement:

```ql
print("Hello", "World", sep=", ", end="!")
```

Here's how the function would parse it:

1. It detects the opening parenthesis `(` and skips any newlines.
2. It parses three expressions: `"Hello"`, `"World"`, and `"sep=", "end="`.
3. When it encounters the keyword `sep`, it consumes the value `"="` and then the string `" , "`, setting `sep` to `", "`.
4. Similarly, when it encounters the keyword `end`, it consumes the value `"="` and then the string `"!"`, setting `end_str` to `"!"` and overriding the default behavior of appending a newline.
5. After processing all expressions and keyword arguments, it expects a closing parenthesis `)` and constructs an AST node with the parsed expressions and their corresponding keyword values.

### Edge Cases

- **Missing Parentheses**: If the print statement lacks parentheses, the function will assume a simple print statement and parse only one expression.
- **Invalid Keyword Arguments**: If a keyword argument is encountered but its value is not a valid expression (e.g., `sep=123`), the function will consume the invalid value but ignore it, continuing with the next argument or expression.
- **Empty Expressions**: If there are empty expressions within the parentheses (e.g., `print("", "")`), they will be included in the AST node as literal strings.

## Interactions With Other Components

The `parsePrintStmt` function interacts closely with the `Tokenizer` component to identify and consume tokens according to the language's grammar rules. It also utilizes the `ParserExpressions` module to parse individual expressions within the print statement.

Upon successful parsing, the function constructs an `ASTNodePtr` object that represents the print statement. This AST node can then be further processed by the compiler's other modules, such as semantic analysis and code generation, to ensure that the print operation is executed correctly during runtime.