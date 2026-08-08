# QpmSemver.cpp

## Overview

`QpmSemver.cpp` is a crucial component of the Quantum Package Manager (QPM) that handles semantic versioning (SemVer). Semantic versioning is a versioning system used to manage software releases, ensuring that each release can be identified as either a major, minor, or patch update. This file contains functions and structures necessary for parsing and comparing SemVer tokens, which are essential for resolving dependencies and managing package versions within the QPM ecosystem.

## Role in Compiler Pipeline

The `QpmSemver.cpp` file operates at a critical stage in the QPM's dependency resolution process. It parses version constraints specified in the package manifest files and compares them with available package versions. This comparison helps determine whether a package meets the required version criteria, facilitating the installation and management of packages according to their SemVer specifications.

## Key Design Decisions and WHY

1. **Token Parsing**: The file includes functions for splitting strings and trimming whitespace, which are fundamental for parsing version tokens. These functions ensure that version strings are correctly broken down into their constituent parts (major, minor, patch, prerelease).

2. **Prerelease Comparison**: The `comparePrereleaseIdent` function provides a mechanism to compare prerelease identifiers. Prerelease versions are those that come before a stable release, such as "alpha", "beta", or "rc". This function ensures that prerelease versions are compared lexicographically, with numeric identifiers having lower precedence than non-numeric ones.

3. **Partial Version Tokens**: The `Partial` structure is designed to handle partial version tokens, such as "1", "1.2", "1.2.3", and "1.2.3-beta.1". This allows for flexible version matching, where certain components of the version may be omitted or replaced with wildcards ("*").

4. **Build Metadata Handling**: The file includes logic to strip build metadata from version tokens. Build metadata is additional information appended to a version string, typically used to identify specific builds or distributions of a package. By removing this metadata, the focus remains on the core version number.

## Major Classes/Functions Overview

### `splitStr`
- **Purpose**: Splits a string based on a specified separator character.
- **Why**: Essential for breaking down version strings into their individual components.

### `trim`
- **Purpose**: Removes leading and trailing whitespace from a string.
- **Why**: Ensures clean input for further processing.

### `isDigits`
- **Purpose**: Checks if a string consists entirely of digits.
- **Why**: Used to validate version components and determine if they should be treated as numeric identifiers.

### `comparePrereleaseIdent`
- **Purpose**: Compares two prerelease identifiers.
- **Why**: Necessary for accurately determining the precedence of prerelease versions over stable versions.

### `Partial`
- **Purpose**: Represents a partial version token, allowing for flexible version matching.
- **Why**: Enables the handling of version constraints that may not specify all components of the version.

### `parsePartial`
- **Purpose**: Parses a version token into its partial form.
- **Why**: Converts version strings into a structured format that can be easily compared and matched against other versions.

## Tradeoffs

1. **Flexibility vs. Complexity**: Allowing for partial version tokens and wildcards increases flexibility but adds complexity to the parsing and comparison logic. This tradeoff balances the need for precise version control with the practicality of specifying flexible version constraints.

2. **Performance vs. Accuracy**: The use of lexicographical comparison for prerelease identifiers simplifies the implementation but may result in less accurate comparisons for complex prerelease scenarios. Balancing performance with accuracy is challenging, especially when dealing with versioning systems that require fine-grained control.

3. **Readability vs. Maintainability**: While the code aims to be readable through clear naming conventions and comments, maintaining the functionality and correctness of the version parsing and comparison logic requires ongoing effort. This tradeoff highlights the importance of thorough testing and documentation.

Overall, `QpmSemver.cpp` plays a vital role in the QPM's ability to manage and resolve package versions efficiently. Its design decisions strike a balance between flexibility, performance, and readability, making it a robust and reliable part of the compiler pipeline.