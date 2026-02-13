# CLAUDE.md - Jupyter & Data Analysis

## Workflow

- **Always ask if unsure** - If multiple approaches exist with different tradeoffs, present all options and let me decide
- Start with high-level overview before implementing
- Show visualizations and summary statistics before deep analysis

## Code Style

### Python (Jupyter)
- Follow PEP 8 / ruff style
- Use assignment expressions: `if (x := foo()) is None:`
- Add type hints for function signatures
- Use descriptive variable names for DataFrames (e.g., `df_users`, not just `df`)
- Add markdown cells to explain analysis steps

### Library Preferences
- pandas for data manipulation
- matplotlib/seaborn for visualization
- numpy for numerical operations

## Helper Functions (shuhai)

Build highly extensible, general helper functions with:
- Optional arguments for flexibility
- Clear docstrings (acceptable in data analysis context)
- Type hints
- Error handling for common data issues

**Categories:**
1. **File readers**: Read different file types (sts, pdq, plogs, etc.)
   - Return clean DataFrames with standardized column names
   - Handle missing data gracefully
   - Include metadata (file path, read timestamp, row count)

2. **Data processors**: Transform raw data for analysis
   - Consistent output formats
   - Preserve data provenance
   - Support chaining operations

## Analysis Structure

1. **Import & Setup**: Load libraries and helper functions
2. **Load Data**: Read files, show basic info (shape, columns, dtypes)
3. **Explore**: Summary stats, distributions, missing values
4. **Clean**: Handle nulls, outliers, type conversions
5. **Analyze**: Main analysis with visualizations
6. **Conclude**: Key findings and next steps

## Visualizations

- Always label axes and add titles
- Use consistent color schemes
- Include legends when needed
- Show sample sizes in titles
- Export important plots as PNG/PDF

## Meta

- Keep this file under 200 lines
- Update as analysis patterns emerge
