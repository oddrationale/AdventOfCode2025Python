# AdventOfCode2025Python

Solutions for [Advent of Code 2025](https://adventofcode.com/2025) written in Python 3.12.

## Running the notebooks
- Install dependencies with [uv](https://github.com/astral-sh/uv): `uv sync`.
- Start Jupyter against the project environment with `uv run --with jupyter jupyter lab` (listens on `http://localhost:8888/lab`).
- Create a `.env` file with `AOC_SESSION=<your session cookie>` so `advent-of-code-data` can download inputs.

## Conventions
- Each puzzle day lives in its own notebook; no shared module is required.
- Use only the standard library plus `python-dotenv` and `advent-of-code-data`.
