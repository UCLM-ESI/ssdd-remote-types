# remote-types Plantilla repositorio

[![Tests](https://github.com/UCLM-ESI/remote-types/actions/workflows/tests.yml/badge.svg)](https://github.com/UCLM-ESI/remote-types/actions/workflows/tests.yml)
[![Linters](https://github.com/UCLM-ESI/remote-types/actions/workflows/linters.yml/badge.svg)](https://github.com/UCLM-ESI/remote-types/actions/workflows/linters.yml)
[![Type checking](https://github.com/UCLM-ESI/remote-types/actions/workflows/typechecking.yml/badge.svg)](https://github.com/UCLM-ESI/remote-types/actions/workflows/typechecking.yml)

Template for the SSDD laboratory 2024-2025

## Installation

To locally install the package, just run

```
pip install .
```

Or, if you want to modify it during your development,

```
pip install -e .
```

## Execution

To run the template server, just install the package and run

```
remotetypes --Ice.Config=config/remotetypes.config
```

## Configuration

This template only allows to configure the server endpoint. To do so, you need to modify
the file `config/remotetypes.config` and change the existing line.

For example, if you want to make your server to listen always in the same TCP port, your file
should look like

```
remotetypes.Endpoints=tcp -p 10000
```

## Running tests and linters locally

If you want to run the tests and/or linters, you need to install the dependencies for them:

- To install test dependencies: `pip install .[tests]`
- To install linters dependencies: `pip install .[linters]`

All the tests runners and linters are configured in the `pyproject.toml`.

## Continuous integration

This repository is already configured to run the following workflows:

- Ruff: checks the format, code style and docs style of the source code.
- Pylint: same as Ruff, but it evaluates the code. If the code is rated under a given threshold, it fails.
- MyPy: checks the types definitions and the usages, showing possible errors.
- Unit tests: uses `pytest` to run unit tests. The code coverage is quite low. Fixing the tests, checking the
    test coverage and improving it will make a difference.

If you create your repository from this template, you will get all those CI for free.

## Slice usage

The Slice file is provided inside the `remotetypes` directory. It is only loaded once when the `remotetypes`
package is loaded by Python. It makes your life much easier, as you don't need to load the Slice in every module
or submodule that you define.

The code loading the Slice is inside the `__init__.py` file.
