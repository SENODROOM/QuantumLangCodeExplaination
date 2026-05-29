# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is responsible for parsing print statements. It handles both simple print statements and more complex ones that may include optional keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**:
  - None
  
- **Return Value**:
  - An `ASTNodePtr` representing the parsed print statement node.

## How It Works

### Parsing Simple Print Statements

If the current token is not an opening parenthesis (`(`), the function assumes a simple print statement and parses the expression following the `print` keyword. The parsed expression is added to the `args` vector.

```cpp
if (!check(TokenType::LPAREN)) {
    args.push_back(parseExpr());
    while (match(TokenType::COMMA))
        args.push_back(parseExpr());
}
```

### Parsing Complex Print Statements

For complex print statements, the function checks if the next token is an opening parenthesis (`(`). If so, it enters a loop to parse each argument within the parentheses. Each argument can be either a positional argument or a keyword argument.

#### Positional Arguments

Positional arguments are parsed using the `parseExpr()` function and added to the `args` vector.

```cpp
else {
    consume(); // eat '('
    skipNewlines();
    while (!check(TokenType::RPAREN) && !atEnd()) {
        args.push_back(parseExpr());
        skipNewlines();
        if (!match(TokenType::COMMA))
            break;
        skipNewlines();
    }
    expect(TokenType::RPAREN, "Expected ')'");
}
```

#### Keyword Arguments

Keyword arguments are detected by checking if the current token is an identifier followed by an assignment operator (`=`). Depending on the keyword, the function consumes the corresponding value:

- **sep**: Consumes a string literal which sets the separator between printed values.
  
- **end**: Consumes a string literal which sets the character(s) printed at the end of the statement. If a string literal is provided, `newline` is set to `false` to override the default behavior of printing a newline.

- **file**: Consumes an expression which specifies the output stream. Currently, this argument is discarded as its handling is not implemented.

- **flush**: Consumes an expression which indicates whether to forcibly flush the output buffer. Like `file`, this argument is also discarded.

```cpp
while (!check(TokenType::RPAREN) && !atEnd()) {
    if (check(TokenType::IDENTIFIER) && 
        (current().value == "sep" || current().value == "end" ||
         current().value == "file" || current().value == "flush") &&
        pos + 1 < tokens.size() && tokens[pos + 1].type == TokenType::ASSIGN) {
        
        std::string kw = consume().value; // eat keyword name
        consume();                        // eat '='
        
        if (kw == "sep") {
            if (check(TokenType::STRING))
                sep = consume().value;
            else
                parseExpr(); // consume but discard non-literal
        } else if (kw == "end") {
            if (check(TokenType::STRING)) {
                end_str = consume().value;
                newline = false; // end= overrides default newline
            } else
                parseExpr();
        } else {
            parseExpr(); // file= / flush= — consume and discard
        }
    } else {
        args.push_back(parseExpr());
    }
    skipNewlines();
    if (!match(TokenType::COMMA))
        break;
    skipNewlines();
}
expect(TokenType::RPAREN, "Expected ')'");
```

## Edge Cases

- **Missing Parentheses**: If a complex print statement is missing a closing parenthesis (`)`), the function will raise an error indicating that a closing parenthesis was expected.
  
- **Non-Literal Values for `sep` and `end`**: If a non-string literal is provided for the `sep` or `end` keyword arguments, the function will still parse the expression but discard it since these arguments should be strings.

## Interactions with Other Components

- **Tokenizer**: The `parsePrintStmt` function relies on the tokenizer to provide the sequence of tokens for parsing. Tokens such as `print`, `sep`, `end`, `file`, `flush`, `(`, `)`, `,`, and string literals are used during parsing.

- **Abstract Syntax Tree (AST)**: After parsing, the function constructs an ASTNodePtr representing the print statement. This AST node includes the list of arguments and any keyword arguments specified.

- **Error Handling**: The function uses the `expect()` method to ensure that certain tokens are present in the correct order. If a required token is missing, an appropriate error message is raised.

Overall, the `parsePrintStmt` function provides robust support for parsing both simple and complex print statements in the Quantum Language, ensuring that all necessary components are correctly identified and handled.