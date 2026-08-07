# `appendTo` Function

## Purpose
The `appendTo` function is a method used in the Quantum Programming Machine (QPM) compiler to serialize JSON data into a string representation. It handles different types of JSON elements such as null, boolean, number, string, array, and object, appending them correctly formatted to an output stream (`out`). This function is crucial for generating valid JSON strings that can be used for communication or storage purposes.

## Parameters
- **`std::string &out`**: A reference to the output string where the serialized JSON will be appended.
- **`int indent`**: The current indentation level, which determines how many spaces should be added before each line to maintain proper formatting.
- **`int depth`**: The current depth within nested structures, used to manage indentation levels appropriately.

## Return Value
This function does not return any value explicitly. Instead, it modifies the `out` parameter by appending the JSON representation of the current element.

## Edge Cases
1. **Empty Array**: If the current element is an empty array (`Type::Array`), the function appends `"[]"` directly to the output string without further processing.
2. **Empty Object**: Similarly, if the current element is an empty object (`Type::Object`), the function appends `"{}"` directly to the output string.
3. **Nested Structures**: For arrays and objects, the function recursively calls itself to handle nested elements, ensuring correct indentation and structure.

## Interactions with Other Components
- **`formatNumber` Function**: When serializing numbers (`Type::Number`), the `formatNumber` function is called to convert the number into a string format suitable for JSON.
- **`escapeInto` Function**: For strings (`Type::String`), the `escapeInto` function is used to properly escape special characters within the string, ensuring it adheres to JSON syntax rules.
- **`newline` Lambda Function**: This lambda function manages the insertion of newlines and appropriate indentation based on the current depth and indentation level, enhancing readability of the generated JSON string.

### Detailed Explanation of Each Case

#### Null
When the type is `Type::Null`, the function simply appends the string `"null"` to the output stream. There's no need for additional formatting since `"null"` is a reserved keyword in JSON.

```cpp
case Type::Null:
    out += "null";
    break;
```

#### Boolean
For boolean values (`Type::Bool`), the function checks whether the boolean is true or false and appends either `"true"` or `"false"` accordingly.

```cpp
case Type::Bool:
    out += bool_ ? "true" : "false";
    break;
```

#### Number
Numbers are handled by calling the `formatNumber` function, which takes care of converting the number into a string format that is safe for JSON serialization.

```cpp
case Type::Number:
    formatNumber(out, num_);
    break;
```

#### String
Strings are escaped using the `escapeInto` function to ensure they contain only valid JSON characters. After escaping, the function appends the string enclosed in double quotes.

```cpp
case Type::String:
    escapeInto(out, str_);
    break;
```

#### Array
Arrays are processed recursively. If the array is empty, the function appends `"[]"`. Otherwise, it starts with `[`, then iterates over each element, appending it with the appropriate indentation and a comma after each element except the last one. Finally, it closes with `]`.

```cpp
case Type::Array:
    if (arr_.empty()) { out += "[]"; break; }
    out += '[';
    for (size_t i = 0; i < arr_.size(); ++i)
    {
        newline(depth + 1);
        arr_[i].appendTo(out, indent, depth + 1);
        if (i + 1 < arr_.size()) out += ',';
    }
    newline(depth);
    out += ']';
    break;
```

#### Object
Objects are also processed recursively. If the object is empty, the function appends `"{}"`. Otherwise, it starts with `{`, then iterates over each key-value pair, appending the key (escaped), followed by `": "` or just `":"`, and then the value with the appropriate indentation and a comma after each pair except the last one. Finally, it closes with `}`.

```cpp
case Type::Object:
    if (obj_.empty()) { out += "{}"; break; }
    out += '{';
    for (size_t i = 0; i < obj_.size(); ++i)
    {
        newline(depth + 1);
        escapeInto(out, obj_[i].first);
        out += indent > 0 ? ": " : ":";
        obj_[i].second.appendTo(out, indent, depth + 1);
        if (i + 1 < obj_.size()) out += ',';
    }
    newline(depth);
    out += '}';
    break;
```

In summary, the `appendTo` function is a versatile method designed to serialize various JSON data types into a readable string format. Its ability to handle nested structures and edge cases like empty arrays and objects ensures that the output is always well-formed and easy to understand.