# Parser.h — The Parser

The parser takes the flat `vector<Token>` from the lexer and produces a tree of `ASTNodePtr` nodes representing the program's structure. It is a hand-written recursive descent parser with a layered descent approach for expressions.

---

## `ParseError`

```cpp
class ParseError : public std::runtime_error {
public:
    int line, col;
    ParseError(const std::string& msg, int l, int c)
        : std::runtime_error(msg), line(l), col(c) {}
};
```

`ParseError` is separate from the runtime `QuantumError` hierarchy in `Error.h` because parse errors occur before any `ASTNode` exists — there is no node to attach a line number to. Instead, `ParseError` carries its own `line` and `col` directly, derived from the offending token. The main entry point in `main.cpp` catches `ParseError` separately from `QuantumError` and formats it differently in the error output.

---

## Public interface

```cpp
class Parser {
public:
    explicit Parser(std::vector<Token> tokens);
    ASTNodePtr parse();
    ...
};
```

The parser takes ownership of the token vector by value. `parse()` consumes the tokens and returns a single `ASTNodePtr` wrapping a `BlockStmt` containing all top-level statements. The choice to return a single root node (rather than `vector<ASTNodePtr>`) means the top-level program has the same representation as any other block, which simplifies the interpreter's entry point.

---

## Token navigation helpers

```cpp
Token& current();           // token at pos
Token& peek(int offset=1);  // token at pos+offset
Token& consume();           // return current, advance pos
Token& expect(TokenType t, const std::string& msg); // consume or throw ParseError
bool   check(TokenType t) const;  // current().type == t, no consume
bool   match(TokenType t);        // if check(t): consume and return true
bool   atEnd() const;       // current().type == EOF_TOKEN
void   skipNewlines();      // consume NEWLINE tokens
```

These seven primitives are the only way the parser reads from the token stream. The distinction between `check` (non-consuming) and `match` (consuming) is the same pattern used by the Crafting Interpreters book and most hand-written parsers — it keeps the lookahead logic local to the decision point rather than spreading advances across multiple methods.

`expect()` is used when a token is grammatically required: `expect(LBRACE, "expected '{'")` either returns the brace token or throws a `ParseError` with the given message and the current token's position. This is the primary error-reporting mechanism — parse errors are produced exactly where the grammar fails, not in a centralized error handler.

`skipNewlines()` is called at the start of most statement-level parse methods, allowing newlines to appear freely between statements without being treated as syntax errors. This makes source formatting flexible — whether a programmer puts opening braces on the same line or the next line, the parser accepts it.

---

## Statement parsing

`parseStatement()` is the dispatcher. It inspects `current()` and calls the appropriate specific parse method:

| Current token(s) | Method called |
|-----------------|--------------|
| `LET` / `CONST` | `parseVarDecl(isConst)` |
| `FN` / `DEF` / `FUNCTION` | `parseFunctionDecl()` |
| `CLASS` | `parseClassDecl()` |
| `IF` | `parseIfStmt()` |
| `WHILE` | `parseWhileStmt()` |
| `FOR` | `parseForStmt()` |
| `RETURN` | `parseReturnStmt()` |
| `PRINT` | `parsePrintStmt()` |
| `INPUT` | `parseInputStmt()` |
| `COUT` | `parseCoutStmt()` |
| `CIN` | `parseCinStmt()` |
| `FROM` / `IMPORT` | `parseImportStmt(isFrom)` |
| `TYPE_INT` etc. | `parseCTypeVarDecl(typeHint)` |
| anything else | `parseExprStmt()` |

`isCTypeKeyword(t)` checks whether a token is one of `TYPE_INT`, `TYPE_FLOAT`, `TYPE_DOUBLE`, `TYPE_CHAR`, `TYPE_STRING`, `TYPE_BOOL`, `TYPE_VOID`, `TYPE_LONG`, `TYPE_SHORT`, or `TYPE_UNSIGNED`. When true, `parseStatement()` calls `parseCTypeVarDecl()` to handle C-style declarations like `int x = 5` or `float* p = &y`.

### `parseBodyOrStatement()`

```cpp
ASTNodePtr parseBodyOrStatement(); // block OR single statement (brace-optional)
```

Several constructs (if bodies, loop bodies, function bodies) can be either a full `{ ... }` block or a single statement without braces, matching JavaScript/Rust convention. `parseBodyOrStatement()` checks whether the next non-newline token is `LBRACE`: if so, it calls `parseBlock()`; otherwise it calls `parseStatement()` and wraps the single result in a `BlockStmt`. This is called from `parseIfStmt()`, `parseWhileStmt()`, `parseForStmt()`, and `parseFunctionDecl()`.

### `parseFunctionDecl()` and `parseParamList()`

```cpp
std::vector<std::string> parseParamList(
    std::vector<bool>* outIsRef = nullptr,
    std::vector<ASTNodePtr>* outDefaultArgs = nullptr
);
```

`parseParamList()` is the most complex helper in the parser. It reads a comma-separated parameter list inside parentheses and simultaneously populates three parallel structures:
- The return value: the parameter name strings
- `outIsRef`: `true` for each parameter declared with `&` (`int& r`)
- `outDefaultArgs`: the default value expression for each parameter, or `nullptr` if none

These three vectors are kept parallel so that index `i` in all three refers to the same parameter. The `FunctionDecl` AST node stores all three. Keeping them parallel rather than using a struct-per-parameter is a deliberate space optimization — most parameters have no default and are not references, so storing booleans and pointers separately avoids padding waste.

### `parseCoutStmt()` and `parseCinStmt()`

```cpp
ASTNodePtr parseCoutStmt(); // cout << x << y << endl
ASTNodePtr parseCinStmt();  // cin >> x >> y
```

These handle C++-style I/O syntax. `parseCoutStmt()` reads a chain of `<<` operators after `cout` and converts it to a `PrintStmt` AST node. `parseCinStmt()` reads a chain of `>>` operators after `cin` and converts it to an `InputStmt`. This means the interpreter only ever sees `PrintStmt` and `InputStmt` nodes — it never needs to handle `cout`/`cin` as special cases. The parser absorbs the syntactic difference between Python-style `print(x)` and C++-style `cout << x`.

---

## Expression parsing — layered descent

The comment in the header says "Pratt-style precedence" but the implementation is actually layered recursive descent: one method per precedence level, each calling the next higher level. The call chain is:

```
parseExpr()
  └─ parseAssignment()       // =  +=  -=  *=  /=  etc.
       └─ parseOr()          // or  ||
            └─ parseAnd()    // and  &&
                 └─ parseBitwise()    // |  ^  &
                      └─ parseEquality()   // ==  !=  ===  !==  is
                           └─ parseComparison()  // <  >  <=  >=
                                └─ parseShift()  // <<  >>
                                     └─ parseAddSub()  // +  -
                                          └─ parseMulDiv()  // *  /  //  %
                                               └─ parsePower()  // **
                                                    └─ parseUnary()   // -  !  ~  &  *  not
                                                         └─ parsePostfix()  // ()  []  .  ->  ++  --
                                                              └─ parsePrimary()
```

Each level calls the level below for its operands, and handles its own operators in a loop. For example, `parseAddSub()` calls `parseMulDiv()`, then loops as long as the current token is `PLUS` or `MINUS`, consuming the operator and calling `parseMulDiv()` again for the right operand. This naturally implements left-associativity.

`parsePower()` implements right-associativity (since `2**3**4` should parse as `2**(3**4)`) by recursing right: rather than looping, it calls itself for the right operand.

`parseAssignment()` also recurses right for the same reason: `a = b = 5` should parse as `a = (b = 5)`.

### `parsePostfix()`

Postfix operators — function calls `()`, indexing `[]`, member access `.`, arrow access `->`, and increment/decrement `++`/`--` — all have the same high precedence and are all left-associative. `parsePostfix()` calls `parsePrimary()` first, then loops consuming any postfix operator it finds, building the AST left-to-right. This handles chained expressions like `obj.method(args)[0].field` correctly.

### `parsePrimary()`

`parsePrimary()` handles the leaves of the expression tree: literals, identifiers, grouped expressions `(expr)`, array literals, dict literals, lambdas, arrow functions, `new` expressions, `super` expressions, template literals, and the address-of `&` and dereference `*` operators (when they appear as prefix unary operators in `parseUnary()`).

### Lambda vs arrow function

Two syntaxes for anonymous functions are supported:

- **Lambda**: `fn(x, y) { return x + y }` — parsed by `parseLambda()`
- **Arrow function**: `(x, y) => x + y` — parsed by `parseArrowFunction(params, ln)` after `parsePrimary()` detects `FAT_ARROW` following a parenthesized parameter list

`parseArrowFunction()` takes the already-parsed parameter list as an argument because by the time the parser sees `=>`, it has already consumed the `(params)` part thinking it might be a grouped expression.

---

## `parseArgList()` vs `parseParamList()`

These are distinct:

- `parseArgList()` — parses **call arguments**: expressions separated by commas, e.g. `f(x+1, y, 42)`. Returns `vector<ASTNodePtr>`.
- `parseParamList()` — parses **function parameters**: name declarations, optionally with `&` for reference and `= expr` for defaults. Returns `vector<string>` plus parallel output vectors.

The distinction matters because at a call site, arguments are expressions. At a declaration site, parameters are names with metadata. Mixing these up would be a significant bug.