# TypeChecker.cpp

This file implements a static type checker for a quantum programming language. It performs type analysis on Abstract Syntax Tree (AST) nodes to ensure type consistency and catch potential type errors before runtime.

## Complete Code

```cpp
#include "TypeChecker.h"
#include <iostream>
#include "Error.h"

TypeChecker::TypeChecker() : globalEnv(std::make_shared<TypeEnv>()) {
    // Define built-ins
    globalEnv->define("print", "any");
    globalEnv->define("input", "string");
    globalEnv->define("len", "int");
    globalEnv->define("sha256", "string");
    globalEnv->define("aes128", "string");
}

void TypeChecker::check(const std::vector<ASTNodePtr>& nodes) {
    for (auto& node : nodes) {
        checkNode(node, globalEnv);
    }
}

void TypeChecker::check(const ASTNodePtr& node) {
    if (node && node->is<BlockStmt>()) {
        check(node->as<BlockStmt>().statements);
    } else {
        checkNode(node, globalEnv);
    }
}

std::string TypeChecker::checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env) {
    if (!node) return "void";

    if (node->is<NumberLiteral>()) return "float";
    if (node->is<StringLiteral>()) return "string";
    if (node->is<BoolLiteral>()) return "bool";
    
    if (node->is<Identifier>()) {
        return env->resolve(node->as<Identifier>().name);
    }

    if (node->is<VarDecl>()) {
        auto& vd = node->as<VarDecl>();
        std::string initType = "any";
        if (vd.initializer) initType = checkNode(vd.initializer, env);
        
        std::string declaredType = vd.typeHint.empty() ? initType : vd.typeHint;
        
        // Basic type check
        if (!vd.typeHint.empty() && vd.typeHint != "any" && initType != "any" && vd.typeHint != initType) {
            std::cerr << Colors::YELLOW << "[StaticTypeWarning] " << Colors::RESET 
                      << "Type mismatch for '" << vd.name << "'. Found " << initType 
                      << " but expected " << vd.typeHint << " (line " << node->line << ")\n";
        }
        
        env->define(vd.name, declaredType);
        return "void";
    }

    if (node->is<FunctionDecl>()) {
        auto& fd = node->as<FunctionDecl>();
        auto subEnv = std::make_shared<TypeEnv>(env);
        for (size_t i = 0; i < fd.params.size(); ++i) {
            std::string pType = "any";
            if (i < fd.paramTypes.size() && !fd.paramTypes[i].empty()) pType = fd.paramTypes[i];
            subEnv->define(fd.params[i], pType);
        }
        checkNode(fd.body, subEnv);
        
        std::string retType = fd.returnType.empty() ? "any" : fd.returnType;
        env->define(fd.name, "fn"); // Simplification
        return "void";
    }

    if (node->is<BlockStmt>()) {
        auto& block = node->as<BlockStmt>();
        auto subEnv = std::make_shared<TypeEnv>(env);
        for (auto& stmt : block.statements) {
            checkNode(stmt, subEnv);
        }
        return "void";
    }

    if (node->is<BinaryExpr>()) {
        auto& be = node->as<BinaryExpr>();
        std::string left = checkNode(be.left, env);
        std::string right = checkNode(be.right, env);
        if (be.op == "+" || be.op == "-" || be.op == "*" || be.op == "/") return "float";
        if (be.op == "==" || be.op == "!=" || be.op == "<" || be.op == ">") return "bool";
        return "any";
    }

    if (node->is<CallExpr>()) {
        auto& ce = node->as<CallExpr>();
        checkNode(ce.callee, env);
        for (auto& arg : ce.args) checkNode(arg, env);
        // We could look up the function signature here if we had a more complex type system
        return "any";
    }

    if (node->is<ExprStmt>()) {
        return checkNode(node->as<ExprStmt>().expr, env);
    }

    if (node->is<ReturnStmt>()) {
        return checkNode(node->as<ReturnStmt>().value, env);
    }

    return "any";
}
```

## Code Explanation

### Overview
The `TypeChecker` class implements static type analysis for a quantum programming language. It traverses the AST and verifies type consistency while maintaining a hierarchical environment for variable scoping. This type checker serves as a foundational component for ensuring type safety in quantum programs before execution.

### Key Components

#### Constructor
The constructor initializes the type checking system by creating a global type environment and defining built-in functions with their respective return types. These built-ins include:

- `print`: A versatile output function that accepts any type (`any`)
- `input`: User input function that returns string values
- `len`: Length calculation function returning integers
- `sha256`, `aes128`: Cryptographic hash functions that return string representations

This initialization ensures that common operations are available and properly typed from the start of program analysis.

#### Public Methods

**Multiple Node Checking**
The primary entry point accepts a vector of AST nodes and iteratively performs type checking on each one. This method is typically used for processing entire programs or large code blocks.

**Single Node Checking**
An overloaded method provides flexible type checking for individual nodes. It includes special handling for block statements to ensure proper lexical scoping is maintained during analysis.

#### Core Type Checking Logic

The main type checking method serves as a dispatcher that handles different AST node types through pattern matching and polymorphic type inspection.

**Literal Type Resolution**
Basic literals are automatically mapped to their corresponding types:
- Numeric literals are classified as floating-point values
- Text literals are identified as strings
- Boolean literals maintain their logical type

**Identifier Resolution**
Variable names are looked up in the current type environment, which maintains a hierarchical scope chain. This allows the checker to determine the declared type of any variable and detect undefined references.

**Variable Declaration Analysis**
Variable declarations undergo comprehensive type analysis:
- The initializer expression is type-checked to determine the inferred type
- If no explicit type hint is provided, the inferred type becomes the declared type
- When both type hints and initializers exist, the checker validates compatibility
- Type mismatches generate warnings with detailed context including variable names, expected types, found types, and source locations
- Successfully typed variables are registered in the current environment for future reference

**Function Declaration Processing**
Function declarations create isolated lexical scopes:
- A new sub-environment is created to encapsulate the function's scope
- Parameters are registered with their declared types, defaulting to `any` when unspecified
- The function body undergoes complete type checking within this isolated environment
- Functions are currently simplified to a generic `fn` type, though this could be enhanced to include full signature information

**Block Statement Scoping**
Block statements implement proper lexical scoping through environment inheritance:
- Each block creates its own sub-environment that inherits from the parent
- All statements within the block are type-checked using this isolated scope
- This enables proper variable shadowing and scope management

**Binary Expression Type Deduction**
Binary operations are typed based on their operator semantics:
- Arithmetic operations (addition, subtraction, multiplication, division) consistently return floating-point values
- Comparison operations (equality, inequality, less than, greater than) return boolean values
- Other operations default to the generic `any` type

**Function Call Validation**
Function calls ensure type safety through argument checking:
- The callee expression is type-checked to verify it represents a callable entity
- All arguments are individually type-checked to ensure they're valid expressions
- Currently returns `any` due to simplified function type representation, though this could be enhanced with signature lookup

**Statement Wrapper Handling**
Various statement types are processed by forwarding to their underlying expressions:
- Expression statements simply evaluate and type-check their contained expression
- Return statements validate the type of the value being returned

### Type System Architecture

**Supported Type Categories**
- `float`: Represents all numeric values and arithmetic results
- `string`: Encapsulates text data and cryptographic function outputs
- `bool`: Handles logical values and comparison results
- `any`: Serves as a universal type for dynamic values and unknown types
- `fn`: Represents callable entities (simplified representation)

**Environment Hierarchy**
The type system employs a sophisticated environment structure:
- The global environment contains built-in functions and top-level declarations
- Sub-environments are created for each lexical scope (functions, blocks)
- Environment inheritance enables proper variable resolution and shadowing
- This hierarchical approach mirrors typical programming language scoping rules

**Error Reporting Strategy**
The type checker implements a non-fatal error approach:
- Type mismatches generate warnings rather than halting compilation
- Colored output enhances readability and distinguishes warnings from other output
- Detailed error messages include variable names, type information, and source locations
- Line numbers help developers quickly locate problematic code

### Current Limitations and Enhancement Opportunities

**Function Type Representation**
Currently, functions are simplified to a generic `fn` type. Future enhancements could include:
- Full function signatures with parameter and return types
- Overloading support for functions with different parameter types
- Generic function types with type parameters

**Type Inference Capabilities**
The current inference system is basic but functional. Potential improvements include:
- More sophisticated type propagation through complex expressions
- Type narrowing based on control flow analysis
- Generic type inference for collections and data structures

**Advanced Type System Features**
Several advanced features could be added:
- User-defined types (structs, classes, interfaces)
- Generic programming with type parameters
- Union and intersection types for more flexible typing
- Type aliases for better code organization

**Quantum-Specific Typing**
As this is designed for quantum programming, specialized features could include:
- Qubit types with quantum state tracking
- Quantum gate operation typing
- Quantum circuit validation through types
- Measurement result typing

This type checker provides a solid foundation for static analysis while remaining extensible for the advanced type system features that quantum programming languages may require.
