# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function is responsible for parsing print statements within the Quantum Language compiler. This function aims to handle both basic print statements and those that include optional keyword arguments like `sep`, `end`, `file`, and `flush`. By correctly interpreting these elements, the function ensures that the print statement is accurately represented in the Abstract Syntax Tree (AST).

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.

- **Return Value**:
  - The function returns an `ASTNodePtr` representing the parsed print statement.

## Workflow

1. **Initialization**: 
   - The function starts by initializing variables to track the line number (`ln`), whether a newline should be added after printing (`newline`), the separator string (`sep`), the end-of-line string (`end_str`), and a vector of AST nodes (`args`) to store the expressions to be printed.

2. **Handling Parentheses**:
   - If the next token is an opening parenthesis (`TokenType::LPAREN`), indicating a complex print statement with keyword arguments, the function consumes the token and skips any newlines until a closing parenthesis or the end of input is encountered.
   
3. **Parsing Keyword Arguments**:
   - Within the parentheses, the function checks for identifier tokens followed by an assignment operator (`=`). These indicate potential keyword arguments (`sep`, `end`, `file`, `flush`). For each detected keyword argument:
     - The keyword name is consumed.
     - The assignment operator is also consumed.
     - Depending on the keyword, the function further processes the following token:
       - For `sep` and `end`, if the next token is a string literal (`TokenType::STRING`), its value is assigned to `sep` or `end_str` respectively. If not, the function calls `parseExpr()` to consume and discard the expression.
       - For `file` and `flush`, the function simply calls `parseExpr()` to consume and discard the expression without using its value.

4. **Parsing Non-Key Argument Expressions**:
   - If the next token is not a keyword argument, the function parses it as an expression using `parseExpr()` and adds the resulting AST node to the `args` vector.
   - Commas are used to separate multiple arguments. The function continues processing subsequent arguments until a comma is no longer found or the end of the parentheses is reached.

5. **Closing Parenthesis**:
   - After parsing all arguments, the function expects a closing parenthesis (`TokenType::RPAREN`). If this token is missing, an error message is generated.

6. **Handling Simple Print Statements**:
   - If the initial token is not an opening parenthesis, the function assumes a simpler print statement with only one or more expressions separated by commas. It parses these expressions into the `args` vector similarly to the previous step.

7. **Finalizing the AST Node**:
   - Once all arguments are processed, the function constructs an `ASTNodePtr` representing the print statement. This node includes the line number (`ln`), the `newline` flag, the `sep`, `end_str`, and the `args` vector.

8. **Edge Cases**:
   - The function gracefully handles cases where keyword arguments are not properly formatted (e.g., missing values or incorrect syntax).
   - It also manages scenarios with multiple expressions separated by commas, ensuring they are correctly grouped into the `args` vector.

9. **Interactions with Other Components**:
   - The `parsePrintStmt` function interacts closely with the lexer to consume tokens and determine their type.
   - It leverages the parser's state machine to manage the flow of tokens and ensure correct parsing of nested structures.
   - Upon successful parsing, the function contributes to building the AST, which is then used by other parts of the compiler for semantic analysis, code generation, and execution.

This comprehensive approach allows the `parsePrintStmt` function to effectively parse various forms of print statements in the Quantum Language, providing a robust foundation for further compilation steps.