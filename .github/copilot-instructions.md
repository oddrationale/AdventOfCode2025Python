## Big picture
- Each puzzle day lives in its own notebook (`Day01.ipynb`, `Day02.ipynb`, etc.) and solutions are written inline rather than in standalone modules.
- The notebooks **strictly favor a functional, immutable style**: puzzle-specific concepts are modeled as `@dataclass(frozen=True, slots=True)` types with helpers such as `from_string`/`rotate` to keep state transitions explicit.
- Parsing logic stays close to the domain model; solving Part 1 and Part 2 means transforming immutable data streams and counting conditions—**never mutating global state**.
- **Avoid imperative code** (explicit loops with mutation, index manipulation) unless it is demonstrably the best approach for performance or clarity. Prefer comprehensions, `map`, `filter`, `itertools`, and `functools.reduce`.

## Workflow essentials
- Fetch inputs with `aocd.get_data(day=<n>, year=2025)` after `python-dotenv` loads credentials from `.env`. Reuse the downloaded string (`puzzle_input`) to avoid repeated API calls.
- Always craft a `sample_input` block copied from the puzzle text and run assertions (`assert count_zero_positions(sample_dials) == 3`) before computing the real answer.
- Keep execution linear: cells generally define data models first, then helpers, then sample validation, and finally the puzzle answer cells (`part1`, `part2`).
- Notebook cells should only import what they use; unused imports trigger Pylance warnings, so move shared imports to the cells that reference them.

## Coding patterns to follow
- Start by modeling the puzzle entities as **immutable dataclasses** (`@dataclass(frozen=True, slots=True)`); validate inputs in `__post_init__` to ensure instances always represent legal puzzle states.
- Because every puzzle input arrives as a raw string, pair each dataclass with a `from_string`/`from_lines` helper that performs strict parsing (guard values, raise on invalid tokens, and use the standard library only).
- **Write small, pure, composable functions**: each function should take inputs, return outputs, and have no side effects. Functions should be easy to test in isolation.
- For aggregations, lean on the Python standard library (`itertools.accumulate`, `collections.Counter`, `functools.reduce`, etc.), making sure reducers return the portion of the state that the next step expects.
- **Prefer declarative over imperative**: use comprehensions, generator expressions, `map()`, `filter()`, and `itertools` instead of explicit `for` loops with mutation.
- **Avoid mutation**: do not modify data structures in place. Return new values instead. Use `tuple`, `frozenset`, and frozen dataclasses to enforce immutability.
- Keep logic composable: helpers should accept parsed inputs and return plain Python values so they can be unit-tested via inline assertions.
- Keep code cells narrowly focused: introduce a single class or helper per cell so execution order stays readable and re-runnable.
- Whenever possible, have each code cell finish by returning or printing a concrete value (often a quick assertion or sample evaluation) so regressions are caught immediately.

### When imperative code is acceptable
Imperative code (explicit loops, mutation) should only be used when:
- Performance is critical and functional overhead is measurable
- The algorithm is inherently stateful (e.g., union-find with path compression, graph traversal with visited sets that must be mutated for efficiency)
- The standard library only provides an imperative API

Even then, isolate imperative code in small, well-named functions and keep the rest of the solution functional.

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
