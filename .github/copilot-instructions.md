# AdventOfCode2025Python – Copilot Instructions

## Big picture
- Each puzzle day lives in its own notebook (`Day01.ipynb`, `Day02.ipynb`, etc.) and solutions are written inline rather than in standalone modules.
- The notebooks favor a functional, immutable style: puzzle-specific concepts are modeled as `@dataclass(frozen=True, slots=True)` types with helpers such as `from_string`/`rotate` to keep state transitions explicit.
- Parsing logic stays close to the domain model; solving Part 1 and Part 2 usually means transforming immutable data streams and counting conditions rather than mutating global state.

## Workflow essentials
- Fetch inputs with `aocd.get_data(day=<n>, year=2025)` after `python-dotenv` loads credentials from `.env`. Reuse the downloaded string (`puzzle_input`) to avoid repeated API calls.
- Always craft a `sample_input` block copied from the puzzle text and run assertions (`assert count_zero_positions(sample_dials) == 3`) before computing the real answer.
- Keep execution linear: cells generally define data models first, then helpers, then sample validation, and finally the puzzle answer cells (`part1`, `part2`).
- Notebook cells should only import what they use; unused imports trigger Pylance warnings, so move shared imports to the cells that reference them.

## Coding patterns to follow
- Start by modeling the puzzle entities as immutable dataclasses; validate inputs in `__post_init__` to ensure instances always represent legal puzzle states.
- Because every puzzle input arrives as a raw string, pair each dataclass with a `from_string`/`from_lines` helper that performs strict parsing (guard values, raise on invalid tokens, and use the standard library only).
- For aggregations, lean on the Python standard library (`itertools.accumulate`, `collections.Counter`, etc.), making sure reducers return the portion of the state that the next step expects.
- Keep logic composable: helpers should accept parsed inputs and return plain Python values so they can be unit-tested via inline assertions.
- Keep code cells narrowly focused: introduce a single class or helper per cell so execution order stays readable and re-runnable.
- Whenever possible, have each code cell finish by returning or printing a concrete value (often a quick assertion or sample evaluation) so regressions are caught immediately.

## External dependencies
- `advent-of-code-data[nb]` handles authentication and downloading puzzle input; ensure `.env` contains `AOC_SESSION`. Do not hardcode credentials.
- `python-dotenv` must be loaded at the top of each notebook to populate environment variables for `aocd`.
- Aside from these two packages, prefer sticking to the Python 3.12 standard library (modern typing features such as `Self`, pattern matching, etc. are available).

## Testing & validation
- Use inline `assert` statements within notebooks for regression checks; replicate the sample explanations from the AoC problem to ensure both parts stay correct.
- When refactoring shared helpers, re-run the sample assertions for both parts before relying on the puzzle answers already printed in later cells.

## Style & conventions
- Maintain the narrative flow from the puzzle description: markdown cells copy the story verbatim, followed by code cells that implement solutions in order.
- Keep answers in variables named `part1`, `part2` so they are easy to find and reuse for submission.
- Avoid side effects beyond printing the final answers; intermediate debugging prints should be removed once logic is verified.
