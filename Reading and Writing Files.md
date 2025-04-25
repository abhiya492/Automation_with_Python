# Comprehensive Notes on Reading and Writing Files in Python

## Table of Contents
1. [Introduction to File Handling](#introduction-to-file-handling)
2. [Files and File Paths](#files-and-file-paths)
   - [Filename and Path](#filename-and-path)
   - [File Extensions](#file-extensions)
   - [Folder Hierarchy](#folder-hierarchy)
3. [Operating System Differences](#operating-system-differences)
   - [Path Separators](#path-separators)
   - [Root Folders](#root-folders)
   - [Case Sensitivity](#case-sensitivity)
4. [Working with Paths](#working-with-paths)
   - [os.path.join()](#ospathjoin)
   - [Current Working Directory](#current-working-directory)
   - [Absolute vs. Relative Paths](#absolute-vs-relative-paths)
5. [File and Directory Operations](#file-and-directory-operations)
   - [Creating Directories](#creating-directories)
   - [The os.path Module](#the-ospath-module)
6. [Reading and Writing Files](#reading-and-writing-files)
   - [Opening Files](#opening-files)
   - [Reading File Contents](#reading-file-contents)
   - [Writing to Files](#writing-to-files)
7. [Saving Variables](#saving-variables)
   - [shelve Module](#shelve-module)
   - [pprint.pformat()](#pprintpformat)
8. [Projects](#projects)
   - [Random Quiz Generator](#random-quiz-generator)
   - [Multiclipboard](#multiclipboard)
9. [Summary](#summary)
10. [Practice Questions](#practice-questions)

## Introduction to File Handling

Variables store data while a program is running, but to persist data after a program closes, we need to save it to files. Files can be thought of as single string values that can be very large (even gigabytes in size).

Key concepts:
- **Filename**: The name of the file (e.g., `projects.docx`)
- **Path**: The location of the file on the computer (e.g., `C:\Users\asweigart\Documents`)

## Files and File Paths

### Filename and Path
A file has two key properties:
1. **Filename**: Usually written as one word (e.g., `projects.docx`)
2. **Path**: Specifies the file's location on the computer

Example:
- Filename: `projects.docx`
- Path: `C:\Users\asweigart\Documents`

### File Extensions
The part after the last period in a filename is the **extension**, which indicates the file type:
- `.docx`: Word document
- `.txt`: Text file
- `.py`: Python script

### Folder Hierarchy
Folders (directories) can contain files and other folders, creating a hierarchy. For example:
```
C:\
└── Users
    └── asweigart
        └── Documents
            └── project.docx
```

## Operating System Differences

### Path Separators
Different operating systems use different path separators:
- **Windows**: Backslash (`\`)
- **OS X/Linux**: Forward slash (`/`)

To write cross-platform code, use `os.path.join()`:
```python
import os
os.path.join('usr', 'bin', 'spam')  # Returns 'usr\\bin\\spam' on Windows
```

### Root Folders
- **Windows**: `C:\` (also called C: drive)
- **OS X/Linux**: `/`
Additional drives appear differently:
- Windows: `D:\`, `E:\`
- OS X: `/Volumes`
- Linux: `/mnt`

### Case Sensitivity
- **Windows/OS X**: Filenames are NOT case-sensitive
- **Linux**: Filenames ARE case-sensitive

## Working with Paths

### os.path.join()
This function creates paths in an OS-independent way:
```python
myFiles = ['accounts.txt', 'details.csv', 'invite.docx']
for filename in myFiles:
    print(os.path.join('C:\\Users\\asweigart', filename))
```
Output:
```
C:\Users\asweigart\accounts.txt
C:\Users\asweigart\details.csv
C:\Users\asweigart\invite.docx
```

### Current Working Directory
Every program has a current working directory (cwd). Functions:
- `os.getcwd()`: Get current working directory
- `os.chdir()`: Change directory

Example:
```python
import os
print(os.getcwd())  # e.g., 'C:\\Python34'
os.chdir('C:\\Windows\\System32')
print(os.getcwd())  # 'C:\\Windows\\System32'
```

### Absolute vs. Relative Paths
- **Absolute path**: Begins with root folder
- **Relative path**: Relative to current working directory

Special names:
- `.`: Current directory
- `..`: Parent directory

Example hierarchy:
```
C:\
└── bacon
    ├── fizz
    │   └── spam.txt
    ├── eggs
    │   └── spam.txt
    └── spam.txt
```

Relative paths when cwd is `C:\bacon`:
- `.\spam.txt` = `C:\bacon\spam.txt`
- `..\spam.txt` = `C:\spam.txt`

## File and Directory Operations

### Creating Directories
Use `os.makedirs()` to create folders recursively:
```python
os.makedirs('C:\\delicious\\walnut\\waffles')
```
Creates all folders in the path if they don't exist.

### The os.path Module
Provides many useful path-related functions:

#### Handling Paths
- `os.path.abspath(path)`: Returns absolute path
- `os.path.isabs(path)`: Checks if path is absolute
- `os.path.relpath(path, start)`: Returns relative path from start

Examples:
```python
os.path.abspath('.')  # 'C:\\Python34'
os.path.isabs('.')  # False
os.path.relpath('C:\\Windows', 'C:\\')  # 'Windows'
```

#### Path Components
- `os.path.dirname(path)`: Everything before last slash
- `os.path.basename(path)`: Everything after last slash
- `os.path.split(path)`: Returns (dirname, basename) tuple

Example:
```python
path = 'C:\\Windows\\System32\\calc.exe'
os.path.dirname(path)  # 'C:\\Windows\\System32'
os.path.basename(path)  # 'calc.exe'
os.path.split(path)  # ('C:\\Windows\\System32', 'calc.exe')
```

#### File Information
- `os.path.getsize(path)`: Returns file size in bytes
- `os.listdir(path)`: Returns list of filenames in directory

Example to calculate total directory size:
```python
totalSize = 0
for filename in os.listdir('C:\\Windows\\System32'):
    totalSize += os.path.getsize(os.path.join('C:\\Windows\\System32', filename))
print(totalSize)
```

#### Path Validation
- `os.path.exists(path)`: True if path exists
- `os.path.isfile(path)`: True if path is a file
- `os.path.isdir(path)`: True if path is a directory

Examples:
```python
os.path.exists('C:\\Windows')  # True
os.path.isfile('C:\\Windows\\System32\\calc.exe')  # True
os.path.isdir('C:\\Windows\\System32\\calc.exe')  # False
```

## Reading and Writing Files

### Opening Files
Three steps:
1. Call `open()` to get a File object
2. Call `read()` or `write()` methods
3. Close the file with `close()`

Modes:
- `'r'`: Read mode (default)
- `'w'`: Write mode (overwrites)
- `'a'`: Append mode

Example:
```python
helloFile = open('C:\\Users\\your_home_folder\\hello.txt')
```

### Reading File Contents
Methods:
- `read()`: Returns entire content as string
- `readlines()`: Returns list of strings (one per line)

Example:
```python
helloContent = helloFile.read()  # 'Hello world!'

sonnetFile = open('sonnet29.txt')
sonnetFile.readlines()
# Returns:
# ["When, in disgrace with fortune and men's eyes,\n",
# " I all alone beweep my outcast state,\n", ...]
```

### Writing to Files
Must open in write (`'w'`) or append (`'a'`) mode:
```python
baconFile = open('bacon.txt', 'w')
baconFile.write('Hello world!\n')  # Returns number of chars written
baconFile.close()

baconFile = open('bacon.txt', 'a')
baconFile.write('Bacon is not a vegetable.')
baconFile.close()

baconFile = open('bacon.txt')
content = baconFile.read()
baconFile.close()
print(content)
# Hello world!
# Bacon is not a vegetable.
```

Note: `write()` doesn't automatically add newlines.

## Saving Variables

### shelve Module
Saves variables to binary files:
```python
import shelve
shelfFile = shelve.open('mydata')
cats = ['Zophie', 'Pooka', 'Simon']
shelfFile['cats'] = cats
shelfFile.close()
```

Later:
```python
shelfFile = shelve.open('mydata')
print(shelfFile['cats'])  # ['Zophie', 'Pooka', 'Simon']
shelfFile.close()
```

### pprint.pformat()
Creates formatted string that can be written to .py file:
```python
import pprint
cats = [{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
fileObj = open('myCats.py', 'w')
fileObj.write('cats = ' + pprint.pformat(cats) + '\n')
fileObj.close()
```

Later can import:
```python
import myCats
print(myCats.cats[0]['name'])  # 'Zophie'
```

## Projects

### Random Quiz Generator
Creates multiple quiz versions with randomized questions and answers.

Key steps:
1. Store states and capitals in dictionary
2. Create quiz and answer key files
3. Shuffle question order
4. Write questions and answers

Example:
```python
import random, os
capitals = {'Alabama': 'Montgomery', 'Alaska': 'Juneau', ...}

for quizNum in range(35):
    # Create quiz and answer key files
    quizFile = open(f'capitalsquiz{quizNum+1}.txt', 'w')
    answerKeyFile = open(f'capitalsquiz_answers{quizNum+1}.txt', 'w')
    
    # Shuffle states
    states = list(capitals.keys())
    random.shuffle(states)
    
    for questionNum in range(50):
        correctAnswer = capitals[states[questionNum]]
        wrongAnswers = list(capitals.values())
        del wrongAnswers[wrongAnswers.index(correctAnswer)]
        wrongAnswers = random.sample(wrongAnswers, 3)
        answerOptions = wrongAnswers + [correctAnswer]
        random.shuffle(answerOptions)
        
        # Write question
        quizFile.write(f'{questionNum+1}. What is the capital of {states[questionNum]}?\n')
        for i in range(4):
            quizFile.write(f"    {'ABCD'[i]}. {answerOptions[i]}\n")
        
        # Write answer key
        answerKeyFile.write(f"{questionNum+1}. {'ABCD'[answerOptions.index(correctAnswer)]}\n")
    
    quizFile.close()
    answerKeyFile.close()
```

### Multiclipboard
Stores multiple clipboard items under keywords.

Key features:
- `save <keyword>`: Saves clipboard to keyword
- `<keyword>`: Loads keyword to clipboard
- `list`: Copies all keywords to clipboard

Implementation:
```python
import shelve, pyperclip, sys

mcbShelf = shelve.open('mcb')

if len(sys.argv) == 3 and sys.argv[1].lower() == 'save':
    mcbShelf[sys.argv[2]] = pyperclip.paste()
elif len(sys.argv) == 2:
    if sys.argv[1].lower() == 'list':
        pyperclip.copy(str(list(mcbShelf.keys())))
    elif sys.argv[1] in mcbShelf:
        pyperclip.copy(mcbShelf[sys.argv[1]])

mcbShelf.close()
```

## Summary
- Files are organized in directories with paths
- `os.path` module handles path operations
- Files can be opened in read, write, or append modes
- `shelve` module persists variables between program runs
- Proper file handling makes programs more robust and user-friendly

## Practice Questions

1. **What is a relative path relative to?**  
   A relative path is relative to the current working directory.

2. **What does an absolute path start with?**  
   An absolute path starts with the root folder (e.g., `C:\` on Windows or `/` on Unix).

3. **What do the os.getcwd() and os.chdir() functions do?**  
   `os.getcwd()` returns the current working directory, while `os.chdir()` changes the current working directory.

4. **What are the . and .. folders?**  
   `.` refers to the current directory, and `..` refers to the parent directory.

5. **In C:\bacon\eggs\spam.txt, which part is the dir name, and which part is the base name?**  
   Dir name: `C:\bacon\eggs`  
   Base name: `spam.txt`

6. **What are the three "mode" arguments that can be passed to the open() function?**  
   `'r'` (read), `'w'` (write), and `'a'` (append).

7. **What happens if an existing file is opened in write mode?**  
   The file is truncated (all contents are deleted) before new content is written.

8. **What is the difference between the read() and readlines() methods?**  
   `read()` returns the entire file content as a single string, while `readlines()` returns a list of strings (one per line).

9. **What data structure does a shelf value resemble?**  
   A shelf value resembles a dictionary, allowing key-value storage that persists to disk.
