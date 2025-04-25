# Comprehensive Notes on Regular Expressions in Python

## Introduction to Regular Expressions

Regular expressions (regex or regexp) are powerful tools for pattern matching and text manipulation. They provide a concise and flexible means for matching strings of text, such as particular characters, words, or patterns of characters.

### Why Use Regular Expressions?

1. **Pattern Matching**: Find specific patterns in text (like phone numbers, emails)
2. **Validation**: Check if input matches a required format
3. **Search and Replace**: Find and modify text patterns
4. **Text Processing**: Extract information from strings or documents

## Basic Regex Syntax in Python

Python's `re` module provides regular expression support. To use it:

```python
import re
```

### Creating Regex Objects

Compile a regex pattern into a Regex object using `re.compile()`:

```python
pattern = re.compile(r'your_pattern_here')
```

The `r` prefix creates a raw string, which treats backslashes as literal characters - essential for regex patterns.

### Basic Matching

The simplest regex matches literal characters:

```python
pattern = re.compile(r'hello')
match = pattern.search('This says hello world!')
print(match.group())  # Output: 'hello'
```

## Regex Metacharacters

### Character Classes

Match specific sets of characters:

- `\d`: Any digit (0-9)
- `\D`: Any non-digit
- `\w`: Any word character (a-z, A-Z, 0-9, _)
- `\W`: Any non-word character
- `\s`: Any whitespace (space, tab, newline)
- `\S`: Any non-whitespace

Examples:
```python
# Find all digits in a string
re.findall(r'\d', 'Room 101')  # ['1', '0', '1']

# Find non-word characters
re.findall(r'\W', 'Hello, world!')  # [',', ' ', '!']
```

### Custom Character Classes

Use square brackets `[]` to create custom character classes:

```python
# Match vowels
vowel_regex = re.compile(r'[aeiouAEIOU]')
vowel_regex.findall('Hello World')  # ['e', 'o', 'o']

# Match hexadecimal digits
hex_regex = re.compile(r'[0-9a-fA-F]')
```

### Negated Character Classes

Use `^` inside `[]` to negate:

```python
# Match non-vowels
non_vowel = re.compile(r'[^aeiouAEIOU]')
non_vowel.findall('Hello')  # ['H', 'l', 'l']
```

### Anchors

- `^`: Start of string
- `$`: End of string

```python
# Match 'Hello' only at start
start_hello = re.compile(r'^Hello')
start_hello.search('Hello world')  # Match
start_hello.search('Say Hello')   # No match

# Match 'world' only at end
end_world = re.compile(r'world$')
end_world.search('Hello world')  # Match
end_world.search('world peace')  # No match
```

### Wildcard Character

The dot `.` matches any character except newline:

```python
at_regex = re.compile(r'.at')
at_regex.findall('The cat in the hat sat.')  # ['cat', 'hat', 'sat']
```

## Repetition in Regex

### Basic Quantifiers

- `*`: 0 or more
- `+`: 1 or more
- `?`: 0 or 1 (optional)
- `{n}`: Exactly n times
- `{n,}`: n or more times
- `{n,m}`: Between n and m times

Examples:

```python
# Match 'a' followed by 0 or more 'b's
re.compile(r'ab*').findall('a ab abb abbb')  # ['a', 'ab', 'abb', 'abbb']

# Match 'a' followed by 1 or more 'b's
re.compile(r'ab+').findall('a ab abb abbb')  # ['ab', 'abb', 'abbb']

# Match 'Mr' or 'Mrs'
re.compile(r'Mrs?').findall('Mr Smith and Mrs Jones')  # ['Mr', 'Mrs']

# Match exactly 3 digits
re.compile(r'\d{3}').findall('123 4567 890')  # ['123', '456', '890']
```

### Greedy vs Non-Greedy Matching

By default, quantifiers are greedy (match as much as possible). Add `?` to make them non-greedy (match as little as possible):

```python
# Greedy matching
greedy_regex = re.compile(r'<.*>')
greedy_regex.search('<tag>content</tag>').group()  # '<tag>content</tag>'

# Non-greedy matching
nongreedy_regex = re.compile(r'<.*?>')
nongreedy_regex.search('<tag>content</tag>').group()  # '<tag>'
```

## Grouping and Capturing

Parentheses `()` create capturing groups:

```python
phone_regex = re.compile(r'(\d{3})-(\d{3}-\d{4})')
match = phone_regex.search('My number is 415-555-4242')
match.group(1)  # '415'
match.group(2)  # '555-4242'
match.group()   # '415-555-4242'
```

### Non-Capturing Groups

Use `(?:...)` for grouping without capturing:

```python
# Only captures area code
phone_regex = re.compile(r'(\d{3})-(?:\d{3})-(\d{4})')
match = phone_regex.search('415-555-4242')
match.groups()  # ('415', '4242')
```

### Named Groups

Assign names to groups with `(?P<name>...)`:

```python
phone_regex = re.compile(r'(?P<area>\d{3})-(?P<exchange>\d{3})-(?P<number>\d{4})')
match = phone_regex.search('415-555-4242')
match.group('area')     # '415'
match.groupdict()       # {'area': '415', 'exchange': '555', 'number': '4242'}
```

## Alternation with the Pipe

The `|` acts like a logical OR:

```python
hero_regex = re.compile(r'Batman|Tina Fey')
hero_regex.search('Batman and Tina Fey').group()  # 'Batman'
hero_regex.search('Tina Fey and Batman').group()  # 'Tina Fey'
```

## Lookahead and Lookbehind Assertions

### Positive Lookahead (`?=`)

Matches if followed by pattern:

```python
# Match 'Isaac' only if followed by ' Asimov'
re.compile(r'Isaac (?=Asimov)').search('Isaac Asimov')  # Match
re.compile(r'Isaac (?=Asimov)').search('Isaac Newton')  # No match
```

### Negative Lookahead (`?!`)

Matches if not followed by pattern:

```python
# Match 'Isaac' only if not followed by ' Asimov'
re.compile(r'Isaac (?!Asimov)').search('Isaac Newton')  # Match
```

### Positive Lookbehind (`?<=`)

Matches if preceded by pattern:

```python
# Match 'Asimov' only if preceded by 'Isaac '
re.compile(r'(?<=Isaac )Asimov').search('Isaac Asimov')  # Match
```

### Negative Lookbehind (`?<!`)

Matches if not preceded by pattern:

```python
# Match 'Asimov' only if not preceded by 'Isaac '
re.compile(r'(?<!Isaac )Asimov').search('Mr. Asimov')  # Match
```

## Regex Flags

Modify regex behavior with flags:

- `re.IGNORECASE` or `re.I`: Case-insensitive matching
- `re.DOTALL` or `re.S`: Makes `.` match newlines
- `re.MULTILINE` or `re.M`: Multi-line matching
- `re.VERBOSE` or `re.X`: Allows comments and whitespace

```python
# Case-insensitive matching
re.compile(r'hello', re.I).search('HELLO').group()  # 'HELLO'

# Dot matches newline
re.compile(r'.*', re.DOTALL).search('line1\nline2').group()  # 'line1\nline2'

# Verbose mode with comments
phone_regex = re.compile(r'''
    (\d{3})     # area code
    -           # separator
    (\d{3})     # exchange
    -           # separator
    (\d{4})     # number
''', re.VERBOSE)
```

## Common Regex Methods

### search()

Finds first match anywhere in string:

```python
match = re.search(r'\d+', 'Answer: 42')
match.group()  # '42'
```

### match()

Matches only at beginning of string:

```python
re.match(r'\d+', '123 abc')  # Match
re.match(r'\d+', 'abc 123')  # No match
```

### findall()

Returns all non-overlapping matches:

```python
re.findall(r'\d+', '12 drummers, 11 pipers, 10 lords')  # ['12', '11', '10']
```

### finditer()

Returns iterator of match objects:

```python
for match in re.finditer(r'\d+', '12 drummers, 11 pipers'):
    print(match.group())
# Output:
# 12
# 11
```

### sub()

Replaces matches with replacement string:

```python
re.sub(r'\d+', 'X', '12 drummers, 11 pipers')  # 'X drummers, X pipers'
```

### subn()

Like sub() but returns tuple (new_string, number_of_subs):

```python
re.subn(r'\d+', 'X', '12 drummers, 11 pipers')  # ('X drummers, X pipers', 2)
```

## Practical Regex Examples

### Email Validation

```python
email_regex = re.compile(r'''(
    [a-zA-Z0-9._%+-]+      # username
    @                      # @ symbol
    [a-zA-Z0-9.-]+         # domain name
    (\.[a-zA-Z]{2,4})      # dot-something
)''', re.VERBOSE)
```

### URL Extraction

```python
url_regex = re.compile(r'https?://(?:[-\w.]|(?:%[\da-fA-F]{2}))+')
```

### Date Validation (MM/DD/YYYY)

```python
date_regex = re.compile(r'''
    (0[1-9]|1[0-2])        # month (01-12)
    /                       # separator
    (0[1-9]|[12][0-9]|3[01]) # day (01-31)
    /                       # separator
    \d{4}                  # year (4 digits)
''', re.VERBOSE)
```

### Strong Password Detection

```python
# At least 8 chars, uppercase, lowercase, and digit
password_regex = re.compile(r'''
    ^                       # start anchor
    (?=.*[A-Z])             # at least one uppercase
    (?=.*[a-z])             # at least one lowercase
    (?=.*\d)                # at least one digit
    .{8,}                   # at least 8 chars
    $                       # end anchor
''', re.VERBOSE)
```

## Common Pitfalls and Tips

1. **Overusing Regex**: Sometimes simple string methods are better
2. **Catastrophic Backtracking**: Avoid complex nested quantifiers
3. **Readability**: Use verbose mode for complex patterns
4. **Testing**: Always test with various input cases
5. **Performance**: Compile patterns if used repeatedly

## Advanced Topics

### Unicode Matching

```python
# Match any word character including Unicode
re.compile(r'\w+', re.UNICODE).findall('café naïve')  # ['café', 'naïve']
```

### Atomic Grouping

Prevent backtracking with `(?>...)`:

```python
re.compile(r'(?>a|ab)c').match('abc')  # No match (atomic group prevents backtracking)
```

### Conditional Matching

```python
# Match 'Mr' or 'Mrs' followed by appropriate pattern
re.compile(r'(Mr|Mrs)(?(1)s\b|\b)').match('Mrs')  # Match
```

### Recursive Patterns

Match nested structures like parentheses:

```python
# Simplified nested parentheses matcher
paren_regex = re.compile(r'\(([^()]|(?R))*\)')
```

## Conclusion

Regular expressions are an incredibly powerful tool in text processing. While they can be complex, mastering them enables you to:

1. Perform sophisticated text searches
2. Validate input formats
3. Extract structured data from text
4. Transform text in powerful ways

The key to regex mastery is:
- Understanding the basic building blocks
- Practicing with real-world examples
- Using tools like regex testers to visualize patterns
- Breaking complex patterns into manageable parts

Remember that while regex is powerful, it's not always the best solution - sometimes simpler string methods or parsing libraries may be more appropriate.
