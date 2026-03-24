# Realm Project Memory

## Project State
- MVP implemented on 2026-03-07
- Code review refactor completed on 2026-03-07
- 61 tests passing, ~2500 LOC across 35 files
- 11 seed agents in configs/ directory
- Uses `.venv/` for Python environment (Python 3.14.2)
- Run tests: `.venv/bin/python -m pytest tests/ -v`
- Run server: `.venv/bin/python -m src.main`

## Architecture
- **Three layers**: Realm business logic -> Adapter -> A2A protocol
- Only `src/adapters/` imports a2a-sdk types
- SQLite via aiosqlite with persistent connections + WAL mode
- Matching: prefilter (inverted tag index) -> embeddings (MiniLM) -> evaluator (Claude Haiku) -> orchestrator
- Cache invalidation via config_hash (sha256 of matchable fields)
- Centralized config: src/realm/config.py (model names, constants, env helpers)

## Key Decisions
- No `type`/`compatible_types` on objectives (dropped per agent-config-schema.md)
- Three-phase matching: tag prefilter + embedding ranking + LLM evaluator
- Haiku model used for cost-efficient evaluation
- `python-slugify` for ID generation from names
- Bidirectional prefilter: both agents must pass tag scoring (no one-sided matches)
- Connection pooling: persistent aiosqlite connections with close() on shutdown
- Specific exception handling (anthropic.APIError, json.JSONDecodeError, etc.)

## File Structure Notes
- Config: src/realm/config.py (centralized model names + operational constants)
- Models: src/realm/models.py (RealmAgent, RealmObjective, RealmMatch, MatchingRunStats)
- Registry: src/realm/registry.py (SQLite CRUD with persistent connection + pagination)
- Store: src/realm/store.py (match results + evaluation cache, row_factory, typed stats)
- Matching: src/realm/matching/{prefilter,embeddings,evaluator,orchestrator}.py
- Adapters: src/adapters/{a2a_adapter,a2a_executor,a2a_server}.py
- Dashboard: src/dashboard/app.py + templates/ (build_agent_from_profile helper)
- Entry: src/realm/__main__.py (Starlette app with startup + shutdown hooks)
- Config loader: load_all_configs returns (agents, file_map) tuple
