# qpmsemver

The `qpmsemver` component is an essential part of the Quantum Programming Language (QPL) compiler, designed to handle semantic versioning (SemVer) for package management and dependency resolution. This component provides utilities for parsing SemVer strings, converting them back to string format, and determining if a given version satisfies a specified range.

## Files and Functions

### tryParse

- **Purpose**: Attempts to parse a SemVer string into its constituent parts (major, minor, patch, pre-release, build metadata).
- **Location**: `include/qpm/semver.h`
- **Usage**: Returns a boolean indicating whether the parsing was successful and fills in the parsed version details.

### toString

- **Purpose**: Converts a parsed SemVer version back into its string representation.
- **Location**: `include/qpm/semver.h`
- **Usage**: Takes a parsed version object and returns the corresponding SemVer string.

### SemVerRange

- **Purpose**: Represents a range of Semantic Versions that can be used to specify acceptable versions for dependencies.
- **Location**: `include/qpm/semver_range.h`
- **Usage**: Defines constructors, methods for adding constraints, and checking if a specific version falls within the range.

### satisfies

- **Purpose**: Determines if a given SemVer version satisfies all the constraints defined in a `SemVerRange`.
- **Location**: `src/satisfies.cpp`
- **Usage**: Takes a parsed version and a `SemVerRange`, and returns a boolean indicating satisfaction.

## Overall Flow

1. **Parsing**:
   - A user or the compiler inputs a SemVer string.
   - The `tryParse` function is called to convert the string into a structured version object.

2. **Conversion**:
   - After parsing, the version object may need to be converted back to a string format.
   - The `toString` function takes the parsed version object and outputs the original SemVer string.

3. **Dependency Resolution**:
   - When resolving dependencies, the compiler needs to check if a package version meets certain requirements.
   - A `SemVerRange` object is constructed based on the dependency's version constraints.
   - The `satisfies` function checks if the current package version is within the allowed range.

4. **Error Handling**:
   - If parsing fails, `tryParse` will return false, allowing the compiler to handle the error appropriately.
   - The `satisfies` function also handles cases where the version does not meet the range constraints, returning false in such scenarios.

## Example Usage

```cpp
#include "qpm/semver.h"
#include "qpm/semver_range.h"

int main() {
    // Parsing a SemVer string
    Version version;
    bool success = tryParse("1.2.3-alpha+build", &version);
    if (!success) {
        std::cerr << "Failed to parse version." << std::endl;
        return 1;
    }

    // Converting back to string
    std::string versionStr = toString(version);

    // Creating a SemVer range
    SemVerRange range;
    range.addConstraint(">="); // At least version 1.0.0
    range.addConstraint("<");  // Less than version 2.0.0

    // Checking if the version satisfies the range
    bool isSatisfied = satisfies(version, range);

    std::cout << "Version: " << versionStr << std::endl;
    std::cout << "Is satisfied: " << (isSatisfied ? "Yes" : "No") << std::endl;

    return 0;
}
```

In this example, we demonstrate the basic usage of `qpmsemver` by parsing a version string, converting it back to a string, creating a version range, and checking if the parsed version satisfies the range.

For more detailed information on each function and class, refer to the individual header files (`include/qpm/semver.h` and `include/qpm/semver_range.h`).