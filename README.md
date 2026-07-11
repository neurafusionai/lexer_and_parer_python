 
***

# Python Earley Parser & Lexical Analyzer

A lightweight, modular **Earley Parser** and **Regex-based Lexer** implemented in Python. This engine allows you to define Context-Free Grammars (CFG), tokenize input strings, and generate custom parse trees using lambda-based processing.

[![Python Version](https://img.shields.io/badge/python-2.7%20%2F%203.x-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 🚀 Features

- **Earley Parsing Algorithm:** Efficiently handles all Context-Free Grammars, including recursive and ambiguous rules.
- **Custom Lexer:** Highly configurable regex-based tokenizer with support for ignored tokens (like whitespace) and value transformation.
- **Dynamic Tree Building:** Define how your parse tree is structured directly within the grammar rules using lambda functions.
- **Precedence Control:** Built-in "Anti-lookahead" support to handle operator precedence and resolve grammar conflicts.
- **Error Reporting:** Detailed exceptions including line and column numbers for both lexical and syntax errors.

---

## 🛠 Installation

Simply clone the repository and ensure both `Lexer.py` and `Parser.py` are in your project directory.

```bash
git clone https://github.com/yourusername/python-earley-parser.git
```

---

## 💻 Usage Example

Here is a quick start on how to define a grammar for simple arithmetic expressions.

### 1. Define the Lexer
The lexer uses templates to match strings and convert them into tokens.

```python
from Lexer import temp, lex

lexer_setup = [
    temp('int', r'[1-9][0-9]*', lambda a: int(a)),
    temp('add', r'\+'),
    temp('space', r' +', lambda a: None), # None means ignore
]

tokens = lex("10 + 20", lexer_setup)
```

### 2. Define the Grammar & Parse
Grammar rules map a non-terminal to a list of tokens and a processing function.

```python
from Parser import rule, parse

grammar = {
    'S': [
        rule('S E', 'start', lambda p: p[1])
    ],
    'E': [
        rule('E add T', 'add', lambda p: p[1] + p[3]),
        rule('T', None, lambda p: p[1])
    ],
    'T': [
        rule('T int', 'int', lambda p: p[1])
    ]
}

# Execute the parser
result = parse(grammar, {}, tokens, grammar['S'][0])
print(result) # Output: 30
```

---

## 📖 How it Works

### The Lexer (`Lexer.py`)
Matches the longest possible string against a list of regular expressions. It tracks line and column numbers by scanning for newline characters, making it easy to debug source files.

### The Parser (`Parser.py`)
Implements the **Earley algorithm**, which consists of three main steps:
1.  **Prediction (Closure):** Expanding non-terminals into their possible rules.
2.  **Scanning (Shift):** Matching terminal tokens from the input.
3.  **Completion (Reduction):** Moving past a non-terminal once its underlying rules are fully matched.

### Anti-Lookahead
To handle edge cases in grammar (like the "dangling else" or specific operator precedence), rules can include an `antilookahead` list. If the next token in the stream is in this list, the parser will not reduce the rule.

---

## 📂 Project Structure

- `Lexer.py`: Contains the `Token`, `Token_template`, and `lex` logic.
- `Parser.py`: Contains the Earley implementation, `rule` helper, and `parse` function.

---

## 🤝 Contributing
Contributions are welcome! If you find a bug or have a feature request, please open an issue or submit a pull request.

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

---

**Keywords for SEO:**
*Python Earley Parser, Context-Free Grammar Python, Lexical Analysis, Tokenizer Python, CFG Parser, Syntax Tree Generator, Earley Algorithm Implementation, Python Compiler Tools.*
