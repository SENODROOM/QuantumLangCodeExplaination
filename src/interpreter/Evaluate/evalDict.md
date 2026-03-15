# `evalDict` Function Explanation

The `evalDict` function in the Quantum Language compiler evaluates a dictionary literal and returns a `QuantumValue` containing the evaluated dictionary. This function is crucial for handling dictionary operations within the quantum programming language, ensuring that keys and values are properly computed before being stored in the final dictionary.

## Parameters/Return Value

- **Parameters**:
  - `e`: A reference to a `DictLiteral` object representing the dictionary literal to be evaluated. The `DictLiteral` contains pairs of keys and values that need to be processed.

- **Return Value**:
  - Returns a `QuantumValue` object encapsulating the evaluated dictionary. The dictionary is represented as a shared pointer to a `Dict` object, which stores key-value pairs.

## How It Works

1. **Initialization**: 
   ```cpp
   auto dict = std::make_shared<Dict>();
   ```
   A new shared pointer to a `Dict` object is created to store the evaluated key-value pairs.

2. **Iterating Over Pairs**:
   ```cpp
   for (auto &[k, v] : e.pairs)
   {
       ...
   }
   ```
   The function iterates over each pair in the `DictLiteral`. Each pair consists of a key (`k`) and a value (`v`).

3. **Handling Spread Operator**:
   ```cpp
   if (!k)
   {
       // Spread: {...obj} — copy all fields from obj into dict
       auto spreadVal = evaluate(*v);
       if (spreadVal.isDict())
           for (auto &p : *spreadVal.asDict())
               (*dict)[p.first] = p.second;
       else if (spreadVal.isInstance())
           for (auto &p : spreadVal.asInstance()->fields)
               (*dict)[p.first] = p.second;
       continue;
   }
   ```
   If the key is `nullptr`, it indicates that the current pair is using the spread operator (`...`). The function evaluates the value (`v`) and checks if it is a dictionary or an instance. If it's a dictionary, all its key-value pairs are copied into the main dictionary. If it's an instance, all its fields (which act like dictionary entries) are also copied into the main dictionary.

4. **Evaluating Key and Value**:
   ```cpp
   auto key = evaluate(*k);
   auto val = evaluate(*v);
   ```
   For regular dictionary entries, both the key and the value are evaluated using the `evaluate` method. The key is converted to a string representation using the `toString` method.

5. **Setting Key-Value Pair**:
   ```cpp
   (*dict)[key.toString()] = std::move(val);
   ```
   The evaluated key and value are then set in the dictionary. The `std::move` ensures that the value is moved rather than copied, optimizing memory usage.

6. **Returning the Result**:
   ```cpp
   return QuantumValue(dict);
   ```
   Finally, the function returns a `QuantumValue` object containing the shared pointer to the evaluated dictionary.

## Edge Cases

- **Empty Dictionary Literal**: If the `DictLiteral` is empty, the function will simply create an empty `Dict` and return it.
- **Spread Operator with Non-Dictionary/Instance Values**: If the spread operator is used but the value is not a dictionary or an instance, the function may throw an error or behave unpredictably depending on how the error handling is implemented.
- **Duplicate Keys**: If multiple pairs have the same key, only the last one will be kept in the dictionary due to the nature of the `Dict` class.

## Interactions with Other Components

- **Evaluator Class**: The `evalDict` function relies on the `evaluate` method of the `Evaluator` class to compute the values of keys and values in the dictionary literal. This method handles various types of expressions and literals.
  
- **Dict Class**: The `Dict` class is responsible for storing and managing key-value pairs. The `evalDict` function uses methods of this class to add entries to the dictionary.

- **Memory Management**: By using `std::shared_ptr`, the `evalDict` function manages memory efficiently, preventing memory leaks and allowing for automatic garbage collection when dictionaries are no longer in use.

Overall, the `evalDict` function plays a vital role in evaluating dictionary literals within the Quantum Language compiler, ensuring that the resulting dictionary accurately reflects the intended structure and contents.