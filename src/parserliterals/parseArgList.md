# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments that are enclosed within parentheses. These arguments can include positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures that the arguments are correctly parsed and structured into an AST (Abstract Syntax Tree) node.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: An `std::vector<ASTNodePtr>` containing pointers to the parsed AST nodes representing each argument in the list.

## Edge Cases

1. **Empty Argument List**: If the argument list is empty (i.e., only parentheses without any content), the function should return an empty vector.
2. **Trailing Comma**: The function should handle trailing commas gracefully, ignoring them and not adding an extra `nullptr` or `ASTNode` to the result.
3. **Nested Parentheses**: The function should be able to handle nested parentheses, ensuring that the correct structure is maintained in the AST.
4. **Generator Expressions**: The function should correctly identify and parse generator expressions, which have the form `f(expr for var in iterable)`.
5. **Unpacking Keyword Arguments**: The function should support unpacking of keyword arguments using the `**` syntax, converting them into a special `UnaryExpr` node.

## Interactions with Other Components

The `parseArgList` function interacts closely with several other components of the Quantum Language compiler:

1. **Tokenizer (`tokens`)**: It uses the global tokenizer to access and consume tokens during the parsing process.
2. **Error Handling (`expect`, `consume`, `match`)**: It leverages error-handling functions like `expect`, `consume`, and `match` to ensure that the correct tokens are encountered and consumed at each step of the parsing process.
3. **AST Construction**: It constructs AST nodes using the `std::make_unique` function and pushes them onto the `args` vector. These nodes represent various types of arguments such as expressions, keyword arguments, and generator expressions.
4. **Expression Parsing (`parseExpr`)**: Within the loop, it calls the `parseExpr` function to parse individual expressions that make up the arguments.
5. **Helper Functions (`skipNewlines`, `readVar`)**: It uses helper functions like `skipNewlines` to manage whitespace and `readVar` to extract variables from generator expressions.

Here is a more detailed breakdown of the code snippet provided:

```cpp
{
    expect(TokenType::LPAREN, "Expected '('"); // Ensure the start of the argument list is '('
    std::vector<ASTNodePtr> args; // Vector to store parsed AST nodes
    skipNewlines(); // Skip any leading newlines

    while (!check(TokenType::RPAREN) && !atEnd()) // Loop until ')' is found or end of input
    {
        int argLn = current().line; // Get the line number of the current token
        skipNewlines(); // Skip any newlines after the argument

        // Handle kwargs unpacking: **expr
        if (check(TokenType::POWER)) // Check if the current token is '**'
        {
            int pLn = current().line; // Get the line number of the '**' token
            consume(); // Consume the '**' token
            auto expr = parseExpr(); // Parse the expression following '**'
            // Wrap in a UnaryExpr{"**", ...} so evalCall knows it's a spread
            args.push_back(std::make_unique<ASTNode>(UnaryExpr{"**", std::move(expr)}, pLn)); // Create and push a UnaryExpr node
            skipNewlines(); // Skip any newlines after the expression
            if (!match(TokenType::COMMA)) // If not followed by ',', break the loop
                break;
            skipNewlines(); // Skip any newlines before the next argument
            continue; // Continue to the next iteration
        }

        // Handle keyword argument: name=expr — skip the name= and just use the value
        if (check(TokenType::IDENTIFIER)) // Check if the current token is an identifier
        {
            size_t la = pos + 1; // Lookahead position
            while (la < tokens.size() && tokens[la].type == TokenType::NEWLINE) // Skip any newlines after the identifier
                ++la;
            if (la < tokens.size() && tokens[la].type == TokenType::ASSIGN) // Check if the next token is '='
            {
                consume(); // Consume the identifier
                while (check(TokenType::NEWLINE)) // Skip any newlines between the identifier and '='
                    consume();
                consume(); // Consume the '=' token
                skipNewlines(); // Skip any newlines after '='
            }
        }

        auto expr = parseExpr(); // Parse the expression for the current argument
        skipNewlines(); // Skip any newlines after the expression

        // Handle generator expressions: f(expr for var in iterable)
        if (check(TokenType::FOR)) // Check if the current token is 'for'
        {
            consume(); // Consume the 'for' token
            std::vector<std::string> vars; // Vector to store variables used in the generator expression
            auto readVar = [&]() // Lambda function to read variables
            {
                if (check(TokenType::IDENTIFIER)) // Check if the current token is an identifier
                    vars.push_back(consume().value); // Add the identifier to the vars vector
                else if (isCTypeKeyword(current().type)) // Check if the current token is a C type keyword
                    vars.push_back(consume().value); // Add the keyword to the vars vector
                else
                    vars