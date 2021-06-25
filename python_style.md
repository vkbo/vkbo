# My Python Style Guide

An excellent tool for checking Python code for errors and coding style is `flake8`.
The documentation is available [here](https://flake8.pycqa.org/en/latest/).

I run it in my Python workspaces with the following config which can be put in a `tox.ini` file or a `setup.cfg` file:
```conf
[flake8]
ignore = E221,E226,E228,E241
max-line-length = 99
exclude = docs/*
```

The command line equivalent, with reporting, is:
```bash
flake8 . --count --ignore E221,E226,E228,E241 --max-line-length=99 --show-source --statistics
```

There is a VSCode extension that wraps `flake8` called
[cornflakes-linter](https://marketplace.visualstudio.com/items?itemName=kevinglasson.cornflakes-linter)
that is convenient for real time linting.


## Ignored Errors

The ignored errors are all `pycodestyle` errors, documented [here](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes).

The reasons behind the ignores are listed below. Two of them are due to PEP8 not permitting column alignment as opposed to many
other coding styles (like for instance Go). I find them very useful in regions of bulk value assignments. There's a reason why
tables are more readable than lists, but it shouldn't be overused.

**E221:** multiple spaces before operator  
**Reason:** Column alignment.

**E226:** missing whitespace around arithmetic operator  
**Reason:** This doesn't actually follow the
[PEP8 recommendation](https://www.python.org/dev/peps/pep-0008/#other-recommendations)
of grouping longer equations by operator precedence like `2*a + 3*b` instead of `a * a + 3 * b`.
Generally, don't use spaces around `*`, `/` and `**`, but do use spaces around `+` and `-`.
For appending strings, the spaces can be dropped. Don't use the `+` operator for appending multiple strings.
Use formatting instead.

**E228** missing whitespace around modulo operator  
**Reason:** See reason for E226. Formatting `%` like `/` and `*` should be possible.

**E241:** multiple spaces after ‘,’  
**Reason:** Column alignment.


## GitHub Action YAML

The check can be implemented as a GitHub action:

```yaml
name: flake8

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  checkSyntax:
    runs-on: ubuntu-latest
    steps:
      - name: Python Setup
        uses: actions/setup-python@v1
        with:
          python-version: 3.7
          architecture: x64
      - name: Checkout Source
        uses: actions/checkout@v2
      - name: Install flake8
        run: pip install flake8
      - name: Syntax Error Check
        run: flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
      - name: Coding Style Violations
        run: flake8 . --count --max-line-length=99 --ignore E221,E226,E228,E241 --show-source --statistics
```
