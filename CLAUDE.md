## Workflow

- **Always ask if unsure** - If multiple approaches exist with different tradeoffs, present all options and let me decide
- State assumptions and plan before implementing
- Multiple interpretations? Present them, don't pick silently
- Do surgical changes - match existing patterns
- Search existing examples in codebase first
- Ask to test, check logs, iterate
- Overview the full task at high level before implementing; iterate to add smaller features
- When dealing with numpy arrays or pandas dataframes, always use vectorized operations where possible

## Code Style

- No AI docstrings except for large functions with complex logic (ask first if unsure)

## Database (MongoDB)

- Optimize queries:
  - Use projections and filters to only query necessary data
  - Filter on indexed columns
