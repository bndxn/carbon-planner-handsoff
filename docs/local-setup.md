# Local setup

## Requirements
- Python 3.14

## Install
1. `python3 -m venv .venv`
2. `source .venv/bin/activate`
3. `pip install -e ".[dev]"`

## Verify
- `ruff check .`
- `ruff format --check .`
- `pytest`
