# Winterwolf - Component Rules


# linus stuff
- vectorised operation
- optimize mongo queries
    - projections and filters to only query data that is necessary
    - filter on indexes
- always ask if unsure
    - if there are multiple approaches, each with different payoffs, please present all ideas and let the user decide
- remove unused variables
- typing where necessary
- no AI docstrings, except for decently big functions with slightly complicated logic. ask first in doubt
- style follows PEP or ruff or sth

mongo db
build a directory for commonly used databases, their columns, each of their types, as well as the columns that the db is indexed on.  this is for future reference


explain mode
use diagrams to explain architecture/flow
explain in a high level first, and i will ask if there are any other details to be clarified


jupyter mode
build library in shuhai helper functions that are highly extensible and as general as possible with optional arguments etc.
1. helper functions to read different types of files e.g. sts, pdq, plogs, etc. 
2. process them cleanly into dataframes for further data analysis


## Principles

- State assumptions and plan. If unsure, ASK
- Multiple interpretations? Present them
- When possible, do surgical changes. Match existing patterns
- Every changed line traces to user's request
- if adding to CLAUDE.md, avoid any verbose examples, always keep it under 300 lines

## ALWAYS

1. Status reporter trait + parent `__init__` | Port `4981`, start before async
2. `TableType` specified | `$WMPYTHONDEPLOY` + `-d|--daemon` in shell
3. `uds` local prod, `socket` remote | `exc_info=True` for errors
4. `.pyc` in prod | Remove YOUR unused imports/vars
5. Search existing examples first | Ask to test, check logs, iterate
6. Use assignment expressions (`:=`) when possible: `if (x := foo()) is None:`

## NEVER

1. AI docstrings | Hardcode creds | `typ=None` | Skip full component implementation unless requested
2. `pytest` | Refactor unrelated code | Delete pre-existing dead code
3. Add unrequested features | Pick silently between interpretations
4. Add orthogonal changes to code above/below pipeline without request
5. Always overview the full task on a high level before implementing, iterate to add smaller features
6. Use `print()` or `logger` - ONLY `self._component_logger.<level>(f"msg {var=}")`
