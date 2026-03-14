# TypeChecker Header Documentation

## 📋 Overview

This directory contains the header file for the TypeChecker component, which defines the interface and data structures for static type analysis in Quantum Language.

## 📁 Files

### TypeChecker.h
The main header file containing:
- **StaticTypeError**: Exception class for type-related errors
- **TypeEnv**: Type environment structure for lexical scoping
- **TypeChecker**: Main type checker class interface

## 🔧 Key Components

### StaticTypeError Class
```cpp
class StaticTypeError : public std::runtime_error {
public:
    int line;
    StaticTypeError(const std::string &msg, int l)
        : std::runtime_error(msg), line(l) {}
};
```
- **Purpose**: Exception handling for type errors
- **Features**: Line number tracking for error location
- **Inheritance**: Extends std::runtime_error

### TypeEnv Structure
```cpp
struct TypeEnv {
    std::map<std::string, std::string> vars;
    std::shared_ptr<TypeEnv> parent;
    
    TypeEnv(std::shared_ptr<TypeEnv> p = nullptr) : parent(p) {}
    
    void define(const std::string& name, const std::string& type);
    std::string resolve(const std::string& name);
};
```
- **Purpose**: Type environment for lexical scoping
- **Features**: Variable-to-type mapping with parent chain
- **Methods**: 
  - `define()`: Add variable to environment
  - `resolve()`: Look up variable in scope chain

### TypeChecker Class
```cpp
class TypeChecker {
public:
    TypeChecker();
    void check(const std::vector<ASTNodePtr>& nodes);
    void check(const ASTNodePtr& node);
    std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env);

private:
    std::shared_ptr<TypeEnv> globalEnv;
};
```
- **Purpose**: Main type checking interface
- **Public Methods**:
  - `TypeChecker()`: Constructor with built-in registration
  - `check(nodes)`: Check multiple AST nodes
  - `check(node)`: Check single AST node
  - `checkNode()`: Core type checking logic
- **Private Members**:
  - `globalEnv`: Global type environment

## 🎯 Design Patterns

### Environment Chain Pattern
- **Lexical Scoping**: Variables resolved in nearest enclosing scope
- **Parent Links**: Environment chain for scope traversal
- **Isolation**: Each scope has its own type environment

### Visitor Pattern Interface
- **AST Traversal**: Methods for different AST node types
- **Type Dispatch**: Returns type strings for expressions
- **Extensibility**: Easy to add new node type handlers

## 🔍 Type System

### Supported Types
- **Primitive Types**: `float`, `string`, `bool`, `any`
- **Complex Types**: `fn` (function), `void` (no return)
- **Type Hints**: Optional static type annotations
- **Built-ins**: Pre-registered global function types

### Type Checking Features
- **Variable Declarations**: Type hint vs initializer validation
- **Function Signatures**: Parameter and return type checking
- **Expression Inference**: Type inference for operations
- **Scope Management**: Lexical scoping with environment chains

## 🛡️ Error Handling

### Exception Hierarchy
```cpp
StaticTypeError : std::runtime_error
```
- **Line Information**: Error location tracking
- **Type Context**: Detailed type mismatch information
- **Integration**: Works with overall error system

### Warning System
- **Non-Fatal**: Type warnings don't stop compilation
- **Informative**: Clear messages with variable names and types
- **Location**: Line numbers for error location

## 🔄 Integration

### With Parser
- **Input**: Receives AST from Parser component
- **Timing**: Runs after parsing, before interpretation
- **Dependencies**: Requires AST node definitions

### With Interpreter
- **Output**: Type-validated AST for execution
- **Optimization**: Type information for runtime optimization
- **Safety**: Early error detection before execution

## 📊 Performance

### Memory Management
- **Shared Pointers**: Efficient environment sharing
- **Automatic Cleanup**: RAII for environment destruction
- **Parent Sharing**: Minimal memory duplication

### Lookup Performance
- **Current Scope**: O(1) map lookup
- **Parent Traversal**: O(depth) chain resolution
- **Caching**: Potential for type result caching

## 🔮 Future Enhancements

### Advanced Types
- **Generic Types**: Template-like type parameters
- **Union Types**: Type unions for flexibility
- **Null Safety**: Explicit null/undefined types
- **Type Aliases**: Custom type definitions

### Enhanced Checking
- **Function Overloading**: Multiple signature support
- **Interface Types**: Structural typing
- **Type Inference**: More sophisticated inference
- **Flow Analysis**: Control flow type checking

### IDE Integration
- **Language Services**: Type information for IDEs
- **Autocomplete**: Type-aware completion
- **Refactoring**: Type-safe code transformations
- **Error Highlighting**: Real-time type checking

---

*This header defines the complete interface for Quantum Language's type checking system, providing static analysis capabilities while maintaining the language's dynamic nature.*
