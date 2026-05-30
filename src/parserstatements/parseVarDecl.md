# parseVarDecl

## Description

The `parseVarDecl` function in the Quantum Language compiler's parser module (`src/parser/ParserStatements.cpp`) is responsible for parsing variable declarations. It handles both single and multiple variable declarations, supporting optional type hints and initialization expressions. The function ensures that the syntax of variable declarations adheres to the language rules, throwing errors if unexpected tokens are encountered.

## Parameters

- **None**: The function does not accept any parameters directly. Instead, it relies on the global state managed by the parser, such as the current token being processed.

## Return Value

- **ASTNodePtr**: The function returns an `ASTNodePtr`, which points to an abstract syntax tree node representing the parsed variable declaration. If multiple variables are declared, it returns a `BlockStmt` containing all the individual variable declarations.

## Edge Cases

1. **Single Variable Declaration**:
   - The function can handle a simple variable declaration like `let x = 5`.
   - If the variable type is not explicitly specified, the function assumes a default type based on the context or language rules.

2. **Multiple Variable Declarations**:
   - The function supports multi-variable declarations separated by commas, such as `const W = 60, H = 24`.
   - Each variable in the list must follow the same syntax rules as a single variable declaration.

3. **Missing Initialization**:
   - If the variable declaration does not include an initialization expression, the function sets the initialization pointer to `nullptr`.

4. **Invalid Token Sequence**:
   - The function throws a `ParseError` if the expected token sequence is not met, such as expecting a variable name but encountering a different token.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve the current token and advance through the input stream.
- **Error Handling**: The function interacts with the error handling mechanism to report parsing errors, ensuring the compiler provides meaningful feedback to the user.
- **Abstract Syntax Tree (AST)**: The function constructs an AST by creating nodes for each variable declaration and potentially wrapping them in a `BlockStmt` if multiple declarations are found.

### Example Usage

Here’s how you might use the `parseVarDecl` function within the parser:

```cpp
void Parser::parseStatement()
{
    switch (current().type)
    {
        case TokenType::LET:
        case TokenType::CONST:
        {
            bool isConst = current().type == TokenType::CONST;
            consume(); // eat LET or CONST
            auto varDecl = parseVarDecl(isConst);
            addStatement(std::move(varDecl));
            break;
        }
        // ... other statement types ...
    }
}

ASTNodePtr Parser::parseVarDecl(bool isConst)
{
    int ln = current().line;
    std::string name;
    if (check(TokenType::IDENTIFIER))
        name = consume().value;
    else if (isCTypeKeyword(current().type))
        name = consume().value;
    else
        throw ParseError("Expected variable name (got '" + current().value + "')", current().line, current().col);

    std::string typeHint;
    if (check(TokenType::COLON))
    {
        consume(); // eat :
        if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type))
            typeHint = consume().value;
    }

    ASTNodePtr init;
    if (match(TokenType::ASSIGN))
        init = parseExpr();

    if (check(TokenType::COMMA))
    {
        auto block = std::make_unique<ASTNode>(BlockStmt{}, ln);
        block->as<BlockStmt>().statements.push_back(
            std::make_unique<ASTNode>(VarDecl{isConst, name, std::move(init), typeHint}, ln));
        while (match(TokenType::COMMA))
        {
            skipNewlines();
            if (atEnd() || check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
                break;
            std::string n2;
            if (check(TokenType::IDENTIFIER))
                n2 = consume().value;
            else if (isCTypeKeyword(current().type))
                n2 = consume().value;
            else
                break;
            
            std::string h2;
            if (check(TokenType::COLON))
            {
                consume();
                if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type))
                    h2 = consume().value;
            }

            ASTNodePtr init2;
            if (match(TokenType::ASSIGN))
                init2 = parseExpr();
            block->as<BlockStmt>().statements.push_back(
                std::make_unique<ASTNode>(VarDecl{isConst, n2, std::move(init2), h2}, ln));
        }
        while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
            consume();
        return block;
    }
    else
    {
        return std::make_unique<ASTNode>(VarDecl{isConst, name, std::move(init), typeHint}, ln);
    }
}
```

In this example, `parseVarDecl` is called when a `LET` or `CONST` keyword is encountered, indicating the start of a variable declaration. The function then parses the variable(s), their type hint (if provided), and initialization expression (if present). If multiple variables are declared, they are wrapped into a `BlockStmt`. Finally, the parsed variable declaration(s) are added to the AST using the `addStatement` method.