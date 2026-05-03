# `check` Function

## Overview

The `check` function serves as a central component in the Quantum Language compiler's type checking phase. Its primary role is to validate the syntax and semantics of the source code against the language's defined rules and constraints. The function processes an abstract syntax tree (AST) and ensures that each node adheres to the language's type system.

### Why It Works This Way

The `check` function iterates through each node in the AST using a range-based for loop. For each node, it calls the `checkNode` function, passing the node and the global environment (`globalEnv`) as arguments. This design allows the `check` function to handle different types of nodes efficiently by delegating the specific type-checking logic to the `checkNode` function, which can be specialized based on the node's type.

### Parameters/Return Value

- **Parameters**:
  - `nodes`: A reference to a vector containing pointers to the nodes of the abstract syntax tree (AST).
  - `globalEnv`: A reference to the global environment object, which holds information about the current scope and available variables/types.

- **Return Value**:
  - None. The function performs type checking in-place and may throw exceptions if type errors are detected.

### Edge Cases

1. **Empty AST**: If the AST is empty, the `check` function will not perform any checks and will simply return without doing anything.
2. **Invalid Node Types**: If a node type is encountered that is not recognized or supported by the compiler, the `checkNode` function should handle this gracefully, possibly throwing an exception or logging a warning.
3. **Scope Management**: The `globalEnv` parameter is crucial for managing variable and type scopes. If the `globalEnv` is not properly initialized or updated, it could lead to incorrect type inference or access violations.

### Interactions With Other Components

- **Parser**: The `check` function relies on the parser to construct the AST. If the parser produces an invalid or incomplete AST, the `check` function may fail or produce incorrect results.
- **Symbol Table**: The `globalEnv` contains a symbol table that tracks variable and type declarations. The `check` function uses this symbol table to resolve identifiers and ensure they refer to valid entities.
- **Error Reporting**: During type checking, the `check` function may detect errors such as type mismatches, undeclared variables, or unsupported operations. These errors are typically reported back to the user through error messages or logs.

Here is a more detailed implementation of the `check` function:

```cpp
void TypeChecker::check(const std::vector<AbstractSyntaxTree*>& nodes, GlobalEnvironment& globalEnv) {
    // Iterate through each node in the AST
    for (auto& node : nodes) {
        try {
            // Delegate type checking to the appropriate checkNode function
            checkNode(node, globalEnv);
        } catch (const TypeError& e) {
            // Handle type errors and report them
            std::cerr << "TypeError at line " << node->getLine() << ": " << e.what() << std::endl;
        } catch (const SyntaxError& e) {
            // Handle syntax errors and report them
            std::cerr << "SyntaxError at line " << node->getLine() << ": " << e.what() << std::endl;
        } catch (const Exception& e) {
            // Handle general exceptions and report them
            std::cerr << "Exception at line " << node->getLine() << ": " << e.what() << std::endl;
        }
    }
}

void TypeChecker::checkNode(AbstractSyntaxTree* node, GlobalEnvironment& globalEnv) {
    switch (node->getType()) {
        case NodeType::VariableDeclaration:
            checkVariableDeclaration(static_cast<VariableDeclaration*>(node), globalEnv);
            break;
        case NodeType::Assignment:
            checkAssignment(static_cast<Assignment*>(node), globalEnv);
            break;
        case NodeType::FunctionCall:
            checkFunctionCall(static_cast<FunctionCall*>(node), globalEnv);
            break;
        case NodeType::BinaryOperation:
            checkBinaryOperation(static_cast<BinaryOperation*>(node), globalEnv);
            break;
        default:
            throw UnsupportedNodeType("Unsupported node type: " + node->getTypeName());
    }
}

void TypeChecker::checkVariableDeclaration(VariableDeclaration* decl, GlobalEnvironment& globalEnv) {
    // Check if the variable is already declared in the current scope
    if (globalEnv.isDeclared(decl->getName())) {
        throw VariableRedeclarationError("Variable '" + decl->getName() + "' already declared");
    }

    // Add the variable to the symbol table
    globalEnv.declareVariable(decl->getName(), decl->getType());
}

void TypeChecker::checkAssignment(Assignment* assign, GlobalEnvironment& globalEnv) {
    // Retrieve the type of the left-hand side variable
    auto lhsType = globalEnv.getType(assign->getLhs()->getName());

    // Retrieve the type of the right-hand side expression
    auto rhsType = checkExpression(assign->getRhs(), globalEnv);

    // Ensure the types match
    if (lhsType != rhsType) {
        throw TypeError("Type mismatch in assignment: expected " + lhsType + ", got " + rhsType);
    }
}

void TypeChecker::checkFunctionCall(FunctionCall* call, GlobalEnvironment& globalEnv) {
    // Retrieve the function declaration
    auto funcDecl = globalEnv.getFunction(call->getFuncName());

    // Check the number of arguments
    if (call->getArgs().size() != funcDecl->getNumParams()) {
        throw ArgumentCountMismatchError("Incorrect number of arguments for function '" + call->getFuncName() + "'");
    }

    // Check the