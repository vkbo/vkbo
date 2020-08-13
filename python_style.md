# My Python Style Guide

An excellent tool for checking Python code for errors and coding style is `flake8`.
The documentation is available [here](https://flake8.pycqa.org/en/latest/).

I run it in my Python workspaces with the following config:
```conf
[flake8]
ignore = E203,E221,E226,E241,E251,E261,E266,E302,E305
max-line-length = 99
exclude = docs/*
```

The command line equivalent, with reporting, is:
```bash
flake8 . --count --ignore E203,E221,E226,E241,E251,E261,E266,E302,E305 --max-line-length=99 --show-source --statistics
```

## Ignored Errors

The ignored errors are all `pycodestyle` errors, documented [here](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes).

The reason behind the ignores are listed below. Many of them are due to PEP8 not permitting column alignment as opposed to many
other coding styles. I find them useful in regions of bulk value assignments. There's a reason why tables are more readable than lists.

**E203:** whitespace before ‘:’  
Reason: Column alignment. It is natural to align dictionary columns along the `:` character,

**E221:** multiple spaces before operator  
Reason: Column alignment.

**E226:** missing whitespace around arithmetic operator  
Reason: This doesn't actually follow the PEP8 recommendation of grouping longer equations by operator precedence like `2*a + 3*b` instead of `a * a + 3 * b`.

**E241:** multiple spaces after ‘,’  
Reason: Column alignment.

**E251:** unexpected spaces around keyword / parameter equals  
Reason: Column alignment.

**E261:** at least two spaces before inline comment  
Reason: With syntax highlighting, this one is meaningless.

**E266:** too many leading ‘#’ for block comment  
Reason: I sometimes use multiple `#`s to indicate importance level, like markdown headers.

**E302:** expected 2 blank lines, found 0  
Reason: Applies to classes, and I prefer to end classes with a comment like `# END Class ClassName` to make it easier to se which class just ended. The double lie break is then redundant.

**E305:** expected 2 blank lines after end of function or class  
Reason: I _always_ end a function with a `return`, preferrably indented at function level. The end of the function is then clear.
