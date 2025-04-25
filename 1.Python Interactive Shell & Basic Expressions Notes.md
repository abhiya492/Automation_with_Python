### **Python Interactive Shell & Basic Expressions Notes**

---

#### **1. Launching the Interactive Shell**
- **Using Mu Editor**:
  - **Windows**: Start Menu → Search "Mu" → Open.
  - **macOS**: Applications → Double-click Mu.
  - Create a blank file (`blank.py`) → Run (F5) to open the interactive shell (`>>>` prompt).

---

#### **2. Expressions**
- **Definition**: A combination of values (e.g., `2`, `'Hello'`) and operators (e.g., `+`, `*`) that evaluates to a single value.
  - Example: `2 + 2` evaluates to `4`.
  - Even a single value is an expression: `2` → `2`.

---

#### **3. Operators & Precedence**
- **Math Operators** (Highest to Lowest Precedence):
  
  | Operator | Operation            | Example      | Result  |
  |----------|----------------------|--------------|---------|
  | `**`     | Exponent             | `2 ** 3`     | `8`     |
  | `%`      | Modulus/Remainder    | `22 % 8`     | `6`     |
  | `//`     | Integer Division     | `22 // 8`    | `2`     |
  | `/`      | Division             | `22 / 8`     | `2.75`  |
  | `*`      | Multiplication       | `3 * 5`      | `15`    |
  | `-`      | Subtraction          | `5 - 2`      | `3`     |
  | `+`      | Addition             | `2 + 2`      | `4`     |

- **Order of Operations**: Follows mathematical rules (parentheses override precedence).
  - Example:  
    ```python
    >>> (2 + 3) * 6  # Evaluates to 30
    ```

---

#### **4. Data Types**
- **Common Types**:
  
  | Data Type | Examples                          |
  |-----------|-----------------------------------|
  | `int`     | `-2`, `0`, `42`                  |
  | `float`   | `-1.25`, `3.14`                  |
  | `str`     | `'a'`, `'Hello!'`, `''` (empty)  |

- **Strings**:
  - Enclosed in single quotes (`'...'`).
  - **Concatenation**: `+` joins strings.  
    ```python
    >>> 'Alice' + 'Bob'  # 'AliceBob'
    ```
  - **Replication**: `*` repeats strings (requires integer).  
    ```python
    >>> 'Alice' * 3  # 'AliceAliceAlice'
    ```
  - **Errors**: Mixing types (e.g., `'Alice' + 42`) causes `TypeError`.

---

#### **5. Variables**
- **Assignment**: Store values using `=`.  
  ```python
  spam = 40  # Variable 'spam' holds 40
  ```
- **Naming Rules**:
  - No spaces; letters, numbers, `_` only.
  - Case-sensitive (`spam ≠ SPAM`).
  - Convention: Start with lowercase (e.g., `myVariable`).

---

#### **6. Handling Errors**
- **Common Errors**:
  - **SyntaxError**: Invalid code structure (e.g., `5 +`).
  - **TypeError**: Incorrect type operations (e.g., `'Alice' + 42`).
- **Debugging**: Search error messages online or refer to resources like [Python Error Guide](https://nostarch.com/automatestuff2/).

---

#### **7. Your First Program**
- **Code Example**:
  ```python
  # This program greets the user and asks for their age.
  print('Hello, world!')
  print('What is your name?')
  myName = input()  # Get user input
  print('Nice to meet you, ' + myName)
  print('Name length:', len(myName))
  print('What is your age?')
  myAge = input()
  print('Next year, you will be ' + str(int(myAge) + 1))
  ```
- **Breakdown**:
  - **`print()`**: Outputs text.
  - **`input()`**: Reads user input (always returns a string).
  - **`len()`**: Returns string length.
  - **Type Conversion**:
    - `int()`: Converts to integer (e.g., `int('42') → 42`).
    - `str()`: Converts to string (e.g., `str(42) → '42'`).

---

#### **8. Key Concepts**
- **Expressions vs. Statements**: Expressions evaluate to values; statements perform actions.
- **Whitespace**: Ignored in expressions but used for readability.
- **Comments**: Use `#` for notes (ignored by Python).
- **Best Practices**:
  - Use descriptive variable names.
  - Handle type conversions explicitly (e.g., `str(int(age) + 1`).

---

#### **9. Practice Examples**
- **Math Operations**:
  ```python
  >>> (5 - 1) * ((7 + 1) / (3 - 1))  # 16.0
  ```
- **String Manipulation**:
  ```python
  >>> 'Hello' + '!' * 3  # 'Hello!!!'
  ```

---

**Next Steps**: Experiment in the interactive shell! Test expressions, variables, and error handling to solidify understanding.
