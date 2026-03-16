# `skipNewlines` Function

## Overview
The `skipNewlines` function is a utility method within the Quantum Language compiler's parser component. Its primary role is to consume and ignore any sequence of newline tokens (`TokenType::NEWLINE`) encountered during parsing. This ensures that the parser can continue processing without being affected by unnecessary whitespace or formatting.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: `void`
  
  The function does not return any value; it simply consumes newline tokens as they are encountered.

## How It Works
The function operates in a loop, checking for newline tokens using the `check` method. If a newline token is found, it is consumed using the `consume` method. This process continues until no more newline tokens are detected.

### Why It Works This Way
This approach ensures that all consecutive newline tokens are ignored efficiently. By repeatedly calling `check` and `consume`, the function effectively skips over any number of newlines without requiring additional checks or loops.

## Edge Cases
- **Empty Input Stream**: If there are no tokens left in the input stream, the function will terminate immediately without consuming any tokens.
- **Non-Newline Tokens**: If non-newline tokens are encountered before a newline token, the function will stop at the first non-newline token.
- **Mixed Token Types**: The function will only consume newline tokens and will leave other types of tokens untouched.

## Interactions With Other Components
- **Lexer**: The `skipNewlines` function relies on the lexer to provide the sequence of tokens. It assumes that the lexer has already processed the input source code into individual tokens.
- **Parser State**: After consuming newline tokens, the parser's state remains unchanged except for the current position in the token stream. This allows the parser to continue processing subsequent tokens as if the newlines had never been present.

In summary, the `skipNewlines` function is an essential utility for handling whitespace in the Quantum Language compiler's parser. It simplifies the parsing process by automatically ignoring sequences of newline tokens, ensuring that the parser focuses solely on meaningful language constructs.