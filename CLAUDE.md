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

---

## Examples (search for relevant patterns in repo before implementing large changes/new code from scratch)

| Pattern | Path |
|---|---|
| Standalone | `defi/solanawmt/middle_tier/solana_balance_middle_tier.py` |
| Linker config | `prod_run/defi-secure-solana-frankfurt/jupiterrfq_solver_2/jupiterrfq_solver.json` |
| MD service | `defi/solanawmt/solana_md_feed_service.py` |
| Execution | `defi/evm/execution/uniswapv3_execution_api.py` |
| Postgres | `tcp/db/postgres_db_async_client.py` |
| Script | `defi/scripts/amm/new_pool_insert_evm.py` |

---

## Workflows

**Env:** `source ~/.bashrc && source $WMPYTHONDEPLOY/activate_defi_312`
**Key configs:** `config/prod-pricing-servers.yaml`, `prod_run/_common/wallet_addresses.json`
**Scripts:** `new_currency_insert.py`, `new_pool_insert_evm.py`, `fetch_pool_info.py` (all in `defi/scripts/`)
**Deploy:** `defi-secure-central-london` (central) | GUI steps cannot be automated

---

## Standalone Service
```python
_loop, _logger = InitialiseComponent(
    component_name="service_name", log_level_name=args.log,
    log_folder=args.log_path, print_to_stdout=False
).loop_and_logger
InitialiseComponent.run_forever(loop=_loop, cb=shut_down)
```

```bash
#!/bin/bash -x
if [ -z "${WMPYTHONDEPLOY}" ]; then if [ -f "build-info.yaml" ]; then export WMPYTHONDEPLOY=$PWD; fi; fi
source $WMPYTHONDEPLOY/.env/bin/activate
if [ -z "$PYTHONPATH" ]; then exit 2; fi
case "${@:$#}" in -d|--daemon) $0 "${@:1:$#-1}" < /dev/null &> /dev/null & disown; exit 0 ;; esac
python3 $WMPYTHONDEPLOY/path/to/service.pyc "$@"
```

---

## Linker Service

```json
{
  "link_type": "tcp_v2", "run_as": "thread",
  "clients": [{"data_format": "json_dictionary", "host": "remote", "port_no": 8702, "msg_broker_type": "socket", "output_channel": false}],
  "servers": [{"data_format": "json_dataclass", "host": "localhost", "port_no": 8513, "msg_broker_type": "uds", "output_channel": true}],
  "wintermute_handler": {
    "status_reporter_component_type": "COMPONENT_TYPE", "status_reporter_restartable_type": "PRODRUN_SUB_FOLDER_SCRIPT",
    "component_location": "tcp.path.to.module", "component_name": "ClassName",
    "singletons": {"defi_mt_client_cache": {"create_fn": "tcp.defi.shared.mt.defi_params_client.create_singleton_defi_mt_client_cache"}}
  }
}
```

```bash
#!/bin/bash -x
case "$1" in -d|--daemon) $0 < /dev/null &> /dev/null & disown; exit 0 ;; esac
if [ -z "${WMPYTHONDEPLOY}" ]; then if [ -f "build-info.yaml" ]; then export WMPYTHONDEPLOY=$PWD; fi; fi
source $WMPYTHONDEPLOY/.env/bin/activate
python3 $WMPYTHONDEPLOY/python_run_files/link-multi-run-v2.pyc -c config.json -l WARNING -lf $HOME/log -pn process_name -prsf "subfolder" "$@"
```

---

## Status Reporter

```python
from tcp.common.status_reporter.status_reporter_component_manager import ComponenStatusGeneralReportTrait, ComponentStatusGeneralAggregateReportTrait
from tcp.common.status_reporter.status_reporter import StatusReporter
from tcp.common.status_reporter.status_reporter_protocol_types import ComponentInternalStatus, ComponentType, RestartableType
from tcp.common.status_reporter.status_reporter_data import ColumnDefnEnum, ColumnDefnStr, ColumnType, TableType

# Single component
class MyService(ComponenStatusGeneralReportTrait):
    def __init__(self, loop, logger):
        ComponenStatusGeneralReportTrait.__init__(self)
    def get_status_and_description(self) -> tuple[ComponentInternalStatus, str]:
        return (ComponentInternalStatus.OK, "ok")

# Aggregate: ComponentStatusGeneralAggregateReportTrait + register_sub_component() / register_capnp_client()

_status_reporter = StatusReporter(loop=_loop, logger=_logger, forwarder_port=4981,
    component_name="name", component_type=ComponentType.YOUR_TYPE,
    summary_cols=[ColumnDefnEnum(ColumnType["Internal Status"], ComponentInternalStatus), ColumnDefnStr(ColumnType["Internal Status Descr"])],
    restartable=RestartableType.PRODRUN_SUB_FOLDER_SCRIPT)
_status_reporter.start()  # BEFORE async init

_table = _status_reporter.create_table(name='T', typ=TableType.CONN_INFO, key_col=..., data_cols=[...])
_table.set_row(key_val=0, data_vals=[...])
```

**Types:** `TableType`: `CONN_INFO`, `DEFI_NODE`, `MD`, `SUMMARY` | `Status`: `OK`, `WARNING`, `ERROR`, `STALE`

**Base classes:** `CreateTaskBase` (async tasks via `self.create_task()`) | `SingletonCache.get_singleton("defi_mt_client_cache")`

---

## Async Pattern

```python
def __init__(self, loop, logger):
    self.__loop = loop
    self.__loop.create_task(self.__startup())

async def __worker(self):
    while True:
        try:
            await self.__do_work()
            await asyncio.sleep(60)
        except asyncio.CancelledError:
            return
        except Exception as e:
            self.__logger.error(f"Error: {e}", exc_info=True)
```

---

## Code Style

- Type hints | Private: `__method()` | `@dataclass(frozen=True, slots=True)`
- unittest only | Imports: stdlib → third-party → local
- Logs: concise, lowercase | Use `--print-log` arg with `print_to_stdout=True` for console output

**Log levels:**
- `debug`: verbose trace, skipped checks, RPC results
- `info`: state changes, tracked events, init steps
- `warning`: alerts, missing data, recoverable issues
- `error`: failures with `exc_info=True`

**TheosCache (standalone):** Requires MongoDB + direct pricing clients | No `event_thread` or `native_pricing_config` (linker-only)

---

**Quirk:** Solana organized by protocol, EVM by function

**Add skill:** `defi/defi-setup/<skill-name>/SKILL.md` → run `setup-symlinks.sh`

**Plugins:** `code-simplifier` available | Hooks: none yet, add to `defi/defi-setup/` if needed

**All Claude config** (skills, hooks, plugins) lives in `defi/defi-setup/` - symlink via `setup-symlinks.sh`
