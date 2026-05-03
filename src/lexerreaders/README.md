# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is an essential part of the Quantum Language (QL) compiler, tasked with reading and parsing tokens from the source code into meaningful elements that can be further processed by the compiler's parser. This component primarily consists of two functions: `readString` and `readIdentifierOrKeyword`.

## Overview

The `lexerreaders` component plays a pivotal role in the lexical analysis phase of the QL compiler. It reads characters from the input source code and identifies them as tokens such as strings, identifiers, keywords, operators, and punctuation marks. These tokens are then passed on to the parser for syntactic analysis.

### Directory Structure

- **readString.cpp**: Contains the implementation of the `readString` function, which specifically handles the parsing of string literals from the source code.
- **readIdentifierOrKeyword.cpp**: Contains the implementation of the `readIdentifierOrKeyword` function, which identifies and categorizes words as either identifiers or reserved keywords.

## Functions

### readString

**Purpose**: Parses string literals from the source code.

**Parameters**:
- `input`: A reference to the input stream containing the source code.
- `token`: A reference to the token object where the parsed string will be stored.

**Returns**:
- `true` if a valid string literal was successfully parsed; otherwise, `false`.

**Description**:
The `readString` function reads characters from the input stream until it encounters the closing quote character (`"`). It ensures that escape sequences within the string are correctly interpreted and stores the resulting string in the provided token object.

### readIdentifierOrKeyword

**Purpose**: Identifies and categorizes words as either identifiers or reserved keywords.

**Parameters**:
- `input`: A reference to the input stream containing the source code.
- `token`: A reference to the token object where the parsed identifier or keyword will be stored.

**Returns**:
- `true` if a valid identifier or keyword was successfully parsed; otherwise, `false`.

**Description**:
The `readIdentifierOrKeyword` function reads characters from the input stream until it encounters a non-alphanumeric character or whitespace. It checks if the word belongs to the set of reserved keywords and categorizes it accordingly. If not, it treats the word as an identifier.

## Flow

1. The lexer begins reading the source code from the input stream.
2. For each character encountered, it determines whether the character forms part of a string literal or an identifier/keyword using the `readString` and `readIdentifierOrKeyword` functions respectively.
3. Once a token is identified, it is stored in the appropriate data structure and passed on to the parser for further processing.
4. This process continues until all characters in the source code have been analyzed and converted into tokens.

By effectively handling the parsing of strings and identifiers/keywords, the `lexerreaders` component ensures that the subsequent stages of the compilation process receive well-formed and categorized tokens, facilitating accurate syntax analysis and error detection.

For more detailed information on the implementation and usage of these functions, refer to the individual `.cpp` files in this directory.