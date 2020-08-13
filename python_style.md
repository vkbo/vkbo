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


