# QpmHttp.h - A Minimal HTTPS Client for Quantum Package Manager

## Role in Compiler Pipeline

The `QpmHttp.h` header file plays a crucial role in the Quantum Package Manager (QPM) by providing a lightweight and efficient way to make HTTP requests. Specifically, it facilitates the retrieval of JSON metadata and downloading tarball bytes from the npm registry. This functionality is essential for fetching package information and assets during the installation process, ensuring that QPM can interact seamlessly with remote repositories.

## Key Design Decisions and WHY

1. **WinHTTP Integration**:
   - **Why**: The decision to use WinHTTP was made due to its integration with the Windows operating system. Unlike libraries like libcurl, which require additional DLL dependencies, WinHTTP is part of the standard library and does not introduce any external overhead. This makes the QPM more portable and easier to deploy across different environments without worrying about missing or incompatible libraries.

2. **Minimal Functionality**:
   - **Why**: By focusing on only the necessary functionalities (GET requests for JSON metadata and tarballs), the QPM avoids unnecessary complexity and potential security risks associated with handling a full-fledged HTTP client. This approach ensures that the core features required for package management are robust and secure.

3. **Automatic Redirection Handling**:
   - **Why**: Implementing automatic redirection handling simplifies the development process and reduces the likelihood of errors related to incorrect URLs. It also aligns with best practices in web development, where following redirects is often expected behavior.

4. **Error Handling**:
   - **Why**: Comprehensive error handling is critical for robust software systems. The `HttpResponse` structure includes fields for status code, response body, and error message, allowing developers to easily identify and handle issues at both the transport level (e.g., DNS failures, TLS errors) and the application level (e.g., invalid responses).

## Major Classes/Functions Overview

### HttpResponse Structure

```cpp
struct HttpResponse
{
    int status = 0;          // HTTP status code
    std::string body;        // Response body content
    std::string error;       // Error message if any
    bool ok() const;         // Returns true if the request was successful
};
```

- **Purpose**: Represents the result of an HTTP request, including the status code, response body, and any encountered errors.
- **Usage**: Used to store and check the outcome of HTTP operations within the QPM.

### httpGet Function

```cpp
HttpResponse httpGet(const std::string &url, const std::string &acceptHeader = "");
```

- **Purpose**: Sends an HTTP GET request to the specified URL and returns the response.
- **Parameters**:
  - `url`: The target URL to send the GET request to.
  - `acceptHeader` (optional): An HTTP header specifying the acceptable content type.
- **Return Value**: An `HttpResponse` object containing the status code, response body, and any errors encountered during the request.

### urlEncodeComponent Function

```cpp
std::string urlEncodeComponent(const std::string &s);
```

- **Purpose**: Percent-encodes a single path component, such as a package name or scope, to ensure it is correctly formatted for use in URLs.
- **Parameters**:
  - `s`: The string to be percent-encoded.
- **Return Value**: The percent-encoded string.

## Tradeoffs

- **Performance vs. Simplicity**: While using WinHTTP provides simplicity and ease of deployment, it may not offer the same performance or feature set as more advanced libraries like libcurl. However, for the specific needs of QPM, this tradeoff is deemed acceptable.
  
- **External Dependencies**: By avoiding external DLL dependencies, QPM becomes more self-contained and easier to distribute. This eliminates the need for users to manage additional libraries, reducing friction in the setup process.
  
- **Feature Set**: The minimalistic approach limits the feature set, but this is balanced against the need for robustness and security. More complex features would increase the attack surface and complicate maintenance.

Overall, `QpmHttp.h` is designed to provide a simple yet effective solution for making HTTP requests within the QPM, leveraging the strengths of WinHTTP while minimizing potential drawbacks.