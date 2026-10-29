---
name: roblox-ddd
description: Use for DDD (Domain-Driven Design in usage on Roblox Studio) architectural pattern for structuring scalable game systems.
---

# DDD (Domain-Driven Design on Roblox Studio)

This skill defines the target architecture. It does not assume any existing project layout. Before placing modules, inspect the current game tree (`search_game_tree`, `script_grep`). If the project already has a structure, follow it. This skill is the reference for new systems or when no convention exists yet.

## Verification
Do not stop until you have verified your own work. Re-check module placement and require boundaries before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase for local variables, member values, functions, params, table keys. PascalCase for types/classes and Roblox API references. LOUD_SNAKE_CASE for `const` bindings holding config or service references. Enum members exempt.
- Services always `const`, never `local`. Module refs, configs, singletons: `const`. Mutable state, loop counters, accumulators: `local`.

## Immutable
- Never delete, edit, or rename bootstrapper scripts: `server/bootstrapper`, `client/bootstrapper`, `client/ui/hud`. These are entry points. Do not touch.
- Never delete, rename, or restructure existing folders in the architecture. Folders are structural contracts. Add new modules inside existing folders, not new folders.
- If a folder or bootstrapper exists, preserve it exactly. Only add or modify modules within allowed boundaries.

## Structure

**Workspace:**
- `Client` — Local rendering. Instances, effects, visuals only visible to local player.
- `Server` — Replication/rendering for all players. Physical instances, effects, visuals visible to everyone.

**ReplicatedStorage/shared:**
- `definitions` — Schemas, contracts, type definitions shared between server/client.
- `index` — Pure `return { ... }` data tables. No methods, no APIs.
- `events` — RemoteEvents, network connections between server/client. Each ModuleScript handles one specific connection (e.g., shop purchase event). Use network library abstractions here.
- `registry` — Data modules returning tables. Annotation/registration environment for votes, records, shared data.
- `packages` — All external libraries (Warp, KoltStore, Spring, etc.). Every dependency lives here.
- `utils` — Pure helper functions shared between server/client.

**ServerScriptService/server:**
- `classes` — Stateful domain objects, server-side classes.
- `controllers` — Orchestrate systems. `function controllerName:start()`. Bootstrap lifecycle.
- `services` — Long-lived modules. Initialized once. `return 0`. No `start()`.
- `utils` — Server-only helpers.
- `bootstrapper` — Entry point. Runs `controllers:start()` and services.

**StarterPlayer/StarterPlayerScripts/client:**
- `classes` — Classes/enums for specific objects. Client-side domain types.
- `controllers` — Bootstrap lifecycle. `function controllerName:start()`.
- `entities` — Client game object representations. Renderable entities (including players). Optimize for client-side rendering and server-client sync strategies.
- `services` — Long-lived modules. `return 0`. No `start()`.
- `utils` — Client-only helpers.
- `bootstrapper` — Entry point.
- `ui/frames` — Dedicated UI modules. Each ModuleScript handles one specific UI (e.g., `shop`, `shop_coin`, `shop_swords`). Code inside is fully dedicated to that UI.
- `ui/helpers` — Quick reusable helpers for UI frames. Interaction logic shared across UI modules.
- `ui/hud` — UI bootstrapper. `return 0`.

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
