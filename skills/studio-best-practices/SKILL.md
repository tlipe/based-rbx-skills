---
name: studio-best-practices
description: "Enforce Roblox Studio engineering standards, architecture, and AI-specific failure prevention. Use when writing, reviewing, extending, debugging, or architecting ANY Roblox Studio code. Prevents auto-creating missing instances, duplicating systems, breaking client/server boundaries, raw RemoteEvents/DataStoreService, and untested code."
---

# Roblox Studio Best Practices

## Verification
Do not stop until you have verified your own work. Playtest + console check before reporting DONE. Never ship untested code.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase for ALL variables/fields/constants/params/table keys — zero tolerance. No UPPER_CASE. PascalCase for types/classes only. Enum members exempt.

## Pre-work
1. Use `search_game_tree`, `script_grep`, `script_search` — never assume structure.
2. Check `ReplicatedStorage`, `ServerScriptService`, `ServerStorage` for existing modules. Extend before creating.
3. If no networking/data library found, stop and report.
4. No `WaitForChild()`/`FindFirstChild()` in ReplicatedStorage paths.
5. Indexes under `shared/index/` are pure data only — wrap queries in `*Helper` under `shared/utils/`.

## Instance rules
Never auto-create missing instances. Detect absence → `error("[System] Missing: path")` → return early.

## Code rules
- Cache `FindFirstChild`/`GetService` at module load, never inside loops/events.
- `const` for immutables: `const maxPlayers = 16`, `const function name(...)`, `const calc = function(...)`.
- No `while true do` — use `RunService.Heartbeat`/`RenderStepped`.
- No `pairs`/`ipairs` — use `for i, v in table do`.
- `table.freeze()` on static tables only when mutation risk is real. One freeze at outermost level.
- String interpolation `` `{var}` `` over `..`.
- No global state (`_G`, `shared`). Disconnect events when done.
- No useless whitespace. Blank lines inside tables only to separate logical groups.
- `require()` at module top-level only. Never inside functions/callbacks. Use intermediate path variables.
- No `.Parent` chains beyond one level. Use `GetService()` refs or cached intermediates.
- Module pattern: `return function(...): ModuleType local self = {} ... return self end`. Never annotate varargs with type.
- Every line must justify its existence. No ritual code.

## Networking
- Never use `RemoteEvent`/`RemoteFunction` directly. Use project library (BridgeNet2, Red, Knit, Warp). If absent: ask user.
- Server always validates; client only reflects confirmed state.

## Data persistence
- Never use `DataStoreService` directly. Use project library (ProfileStore, DataKeep, KoltStore). If absent: ask user.
- Never write player data from client.

## Client / Server
| Client | Server |
|--------|--------|
| Visual, animation, sound, UI | Hitbox, cooldown, data, state, source of truth |

## Testing (when MCP available)
`start_stop_play` → `get_console_output` → fix all errors → stop.

## Utils / Helpers
- Module >200 lines → split. Utils = pure functions, no state. Helpers = contextual logic. Helpers depend on utils, never opposite. One clear purpose per file.
