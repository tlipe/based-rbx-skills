---
name: roblox-ddd
description: Use for DDD (Domain-Driven Design in usage on Roblox Studio) architectural pattern for structuring scalable game systems.
---

# DDD (Domain-Driven Design on Roblox Studio)

## Verification
Do not stop until you have verified your own work. Re-check module placement and require boundaries before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase only. No UPPER_CASE, no snake_case.

## Structure

**Workspace:** Client (local rendering, folder) | Server (replication/rendering for all players, folder)

**ReplicatedStorage/shared:** definitions (data contracts/schemas), index (pure `return { ... }` data tables), events (remotes/abstractions), registry (data modules returning tables), packages (libs: Warp, KoltStore, Spring, etc.), utils (pure functions)

**ServerScriptService/server:** classes (stateful domain objects), controllers (orchestrate systems; `function controllerName:start()`), services (long-lived; initialized once; `return 0`; no start()), utils (server-only helpers), bootstrapper (entry point; runs controllers:start() and services)

**StarterPlayer/StarterPlayerScripts/client:** classes, controllers (`start()`), entities (client game object representations), services (`return 0`; no start()), utils, bootstrapper, ui/frames (UI modules), ui/helpers (UI interaction helpers), ui/hud (UI bootstrapper; `return 0`)

## Conventions
- Controllers: bootstrap lifecycle (`function controllerName:start()`). Services: initialized once, `return 0`, no start().
- Shared modules: deterministic, side-effect free.
- Indexes are data, not APIs. Never attach methods to an index table. Wrap query/filter/lookup in `*Helper` under `shared/utils/`.
- Indexes use direct `return { ... }`. No intermediate variable, no nested `table.freeze`. Mirror sibling shape. No ritual protections without concrete threat.
- Registry/data modules: single expression `return { key = value, ... }`. Never assign to a local first. Never wrap in `table.freeze()`. Freeze is not needed for data contracts; the module boundary already prevents mutation from outside.
- Wrong (do NOT do this):
```lua
local myData = {}
myData.health = 100
myData.name = "sword"
table.freeze(myData)
return table.freeze(myData)
```
- Correct:
```lua
return {
    health = 100,
    name = "sword",
}
```
