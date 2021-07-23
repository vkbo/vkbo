# Python Code Style

When writing Python code, I generally recommend following the PEP8 standard. It is available here:

* Online at [www.python.org/dev/peps/pep-0008](https://www.python.org/dev/peps/pep-0008).
* I've also formatted it as an [Open Document](PEP8.odt), also available as a [PDF](PEP8.pdf).

I personally don't follow PEP8 in all cases, although I am happy to work on projects that do. For
instance, I prefer camelCase to snake_case for function and variable names. I also prefer to let
code lines run to 99 characters rather than 79 if it will prevent line wrapping.

There is also one specific rule that I will sometimes break because I don't fully agree with the
argument made for it, and that is column alignment. That is, when multiple lines of variable
assignments are aligned along the `=` symbol, when large dictionaries are populated, or when
arguments in repeated calls to the same function are aligned. This _can_ make the code easier to
read. I especially do this in the `__init__` function of classes. Many other style guides allow for
it, and for good reason. I do agree that it can be overused, and PEP8 show some good examples of
when not to do this.

## Linting with `flake8`

An excellent tool for checking Python code for errors and coding style is `flake8`.
The documentation is available [here](https://flake8.pycqa.org/en/latest/).

I run it in my Python workspaces with the following config which can be put in a `tox.ini` file or
a `setup.cfg` file:

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

The Python extension in VSCode can be set up for real-time linting with flake8 in the editor.

### Ignored Errors

The ignored errors in the commands above are all `pycodestyle` errors, documented
[here](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes).

Two of the ignored errors are due to the relaxed restriction I generally have on column alignment,
these are the E221 and E241 error codes.

The code E226 is ignored becuse it doesn't actually follow the
[PEP8 recommendation](https://www.python.org/dev/peps/pep-0008/#other-recommendations)
of grouping longer equations by operator precedence like `2*a + 3*b` instead of `a * a + 3 * b`.
Generally, don't use spaces around `*`, `/` and `**`, but _do_ use spaces around `+` and `-`.
The same applies to the modulo operator `%`, hence E228 is also ignored.

### GitHub Action YAML

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
