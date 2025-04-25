**Summary of Boolean Values, Comparison Operators, and Flow Control in Python**

1. **Boolean Values**:
   - Two values: `True` and `False` (capitalized).
   - Used in conditions for decision-making. Avoid syntax errors (e.g., lowercase `true` or assigning `True = ...`).

2. **Comparison Operators**:
   - `==` (equal), `!=` (not equal), `<`, `>`, `<=`, `>=`.
   - Evaluate to `True`/`False`. Example: `42 == '42'` is `False` (type mismatch).

3. **Boolean Operators**:
   - `and`, `or`, `not`.
   - Truth tables define their logic. Order of operations: `not` > `and` > `or`.
   - Combined with comparisons: e.g., `(4 < 5) and (5 < 6)` → `True`.

4. **Flow Control**:
   - **Conditions**: Boolean expressions determining code execution paths.
   - **Blocks**: Indented code grouped under `if`, `elif`, `else`, loops, etc.
   - **`if` Statements**: Execute code if condition is `True`.
   - **`elif`/`else`**: Handle multiple conditions. Order matters (e.g., place specific checks first).
   - **`while` Loops**: Repeat code while condition is `True`. Use `break` to exit, `continue` to skip iterations.
   - **`for` Loops**: Iterate over sequences (e.g., `range(10)`). `range()` parameters: start, stop, step (e.g., `range(0, 10, 2)`).

5. **Key Differences**:
   - `==` vs `=`: Equality check vs assignment.
   - `break` exits loops; `continue` skips to next iteration.

6. **Modules**:
   - Import with `import` (e.g., `import random`).
   - Functions: `random.randint()`, `sys.exit()`.
   - Avoid naming conflicts (e.g., don’t name files after modules).

7. **Example Programs**:
   - **Guess the Number**: Uses loops, comparisons, and `random`.
   - **Rock-Paper-Scissors**: Demonstrates nested loops, user input, and tracking game state.

8. **Common Pitfalls**:
   - Infinite loops (use `CTRL-C` to interrupt).
   - Incorrect indentation breaking block structure.
   - Confusing assignment (`=`) with equality (`==`).

**Practice Questions Answered**:
1. `True` and `False`, capitalized.
2. `and`, `or`, `not`.
3. Truth tables define outputs for all Boolean combinations.
4. Results: `False`, `False`, `True`, `False`, `False`, `True`.
5. `==`, `!=`, `<`, `>`, `<=`, `>=`.
6. `==` checks equality; `=` assigns values.
7. A condition is a Boolean expression used in flow control (e.g., `if`, `while`).
8. Blocks under `if spam == 10`, `if spam > 5`, and `else`.
9. Use `if`/`elif`/`else` to check `spam` values.
10. `CTRL-C` or restart shell.
11. `break` exits loop; `continue` skips to next iteration.
12. All produce 0-9; `range(10)` defaults start=0, step=1.
13. For loop: `for i in range(1,11): print(i)`. While loop: initialize `i=1`, loop while `i <=10`.
14. `spam.bacon()` after `import spam`.

**Extra Credit**:
- `round(number, ndigits)`: Rounds to `ndigits` decimal places.
- `abs(number)`: Returns absolute value.
