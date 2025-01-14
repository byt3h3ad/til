# Scripting with uv

If you want to do some scripting in Python and have to use a dependency, we have the traditional methods of:

1. Running `pip install pandas`, potentially modifying your global environment.

2. Taking the proper route:
    - Create a virtual environment
    - Activate the virtual environment
    - `pip install pandas`
    - Run python

And if you have to use a Python version other than the global one, you end up using `pyenv`, install the version and set that version as local.

So, worst case:

1. `pyenv install 3.12`.

2. `pyenv local 3.12`.

3. `python -m venv .venv`.

4. `source .venv/bin/activate`.

5. `pip install pandas`.

6. `python`.

With `uv`, it’s just 1 command:

```
uv run --python 3.12 --with pandas python
```

[source](https://valatka.dev/2025/01/12/on-killer-uv-feature.html)

