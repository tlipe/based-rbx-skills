---
name: roblox-best-practices
description: "Enforce Roblox Studio engineering standards, architecture, and AI-specific failure prevention. Use when writing, reviewing, extending, debugging, or architecting ANY Roblox Studio code. Prevents auto-creating missing instances, duplicating systems, breaking client/server boundaries, raw RemoteEvents/DataStoreService, and untested code."
---

# Roblox Studio Best Practices

## Verification
Do not stop until you have verified your own work. Playtest + console check before reporting DONE. Never ship untested code.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase for local variables, member values, functions, params, table keys. PascalCase for types/classes and Roblox API references. LOUD_SNAKE_CASE for `const` bindings that hold config or service references. Enum members exempt.
- Services always `const`, never `local`. Example: `const HttpService = game:GetService("HttpService")`.
- Module-level singletons and references that never reassign: `const`.
- Loop counters, accumulators, mutable state: `local`.

## Pre-work
1. Use `search_game_tree`, `script_grep`, `script_search` — never assume structure.
2. Check `ReplicatedStorage`, `ServerScriptService`, `ServerStorage` for existing modules. Extend before creating.
3. If no networking/data library found, stop and report.
4. No `WaitForChild()`/`FindFirstChild()` in ReplicatedStorage paths.
5. Indexes under `shared/index/` are pure data only — wrap queries in `*Helper` under `shared/utils/`.

## Instance rules
Never auto-create missing instances. Detect absence → `error("[System] Missing: path")` → return early.

## Luau syntax enforcement
Follow api-safety skill for full list. Key rules:
- `const` for immutable bindings (services, configs, module refs). `local` for mutable state.
- Generalized iteration (`for k, v in tbl do`) over `pairs`/`ipairs`.
- String interpolation (`` `{var}` ``) over concatenation.
- Compound assignment (`+=`, `..=`, etc.) over verbose reassignment.
- If-then-else expressions over `a and b or c` ternary idiom.
- `continue` for loop skip, not nested if blocks.
- Floor division (`//`) over `math.floor(a / b)`.

## Code rules
- Cache `FindFirstChild`/`GetService` at module load with `const`, never inside loops/events.
- No `while true do` — use `RunService.Heartbeat`/`RenderStepped`.
- `table.freeze()` on static tables only when mutation risk is real. One freeze at outermost level. Never freeze inside data module returns; use direct `return { ... }`.
- No global state (`_G`, `shared`). Disconnect events when done.
- No useless whitespace. Blank lines inside tables only to separate logical groups.
- `require()` at module top-level only, assigned to `const`. Never inside functions/callbacks.
- No `.Parent` chains beyond one level. Use `GetService()` refs or cached intermediates.
- Module pattern: `return function(...): ModuleType local self = {} ... return self end`. Never annotate varargs with type.
- Every line must justify its existence. No ritual code. No defensive wrappers without concrete threat.

## Networking
- Never use `RemoteEvent`/`RemoteFunction` directly. Use project library (BridgeNet2, Red, Knit, Warp). If absent: ask user.
- Server always validates; client only reflects confirmed state.
- Never trust client-supplied data. Validate type, range, ownership server-side before acting.

## Data persistence
- Never use `DataStoreService` directly. Use project library (ProfileStore, DataKeep, KoltStore). If absent: ask user.
- Never write player data from client.
- Wrap all DataStore calls in pcall. Log failures. Retry with backoff on throttle.

## Client / Server
| Client | Server |
|--------|--------|
| Visual, animation, sound, UI | Hitbox, cooldown, data, state, source of truth |

## Testing (when MCP available)
`start_stop_play` → `get_console_output` → fix all errors → stop.

## Utils / Helpers
- Module >200 lines → split. Utils = pure functions, no state. Helpers = contextual logic. Helpers depend on utils, never opposite. One clear purpose per file.
