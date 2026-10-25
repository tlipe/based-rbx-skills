---
name: luau
description: Luau language conventions for Roblox Studio code generation. Auto-loaded; applies to all Luau code written by any agent.
---

# Luau Conventions

SOUL.md covers camelCase, module top order, no elseif chains, Warp/KoltStore boundaries. This skill adds Luau-specific guidance.

## Verification
Do not stop until you have verified your own work. Re-read written code and validate via playtest/console before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase for EVERY identifier — zero tolerance. No UPPER_CASE, no snake_case, no PascalCase variables. Types/classes are the sole PascalCase exception. Enum members (`Enum.*`) exempt.

## Style
- Max performance + minimal size + readability. Modular, safe, no redundancy. Early return.
- Reassess constantly: can it be reduced? Made more direct?
- No `pairs`/`ipairs` — use `for i, v in table do`.
- Module util structure: `return function(...main params) local self = {} function self:method(...) end return self end`.
- Prefer string interpolation `` `{var}` `` over `..`.
- No IIFE. No unnecessary spaces.
- `const` for all immutable variables (`const myVar = 5`). No `local` for immutables.

## Modern Require Syntax

Roblox supports direct path-based requires:

```lua
-- @self: relative to current script
const myModule = require("@self/myModule")
const subModule = require("@self/sub/module")

-- Relative paths: ./ (same folder), ../ (parent folder)
const sibling = require("./siblingModule")
const parentModule = require("../parentModule")
const cousin = require("../otherFolder/cousinModule")

-- @game: absolute path from game root
const warp = require("@game/ReplicatedStorage/KoltFM/get/Packages/Warp")
const signal = require("@game/ReplicatedStorage/KoltFM/get/Packages/Signal")
const guiService = require("@game/ReplicatedStorage/KoltFM/get/Services/GuiService")

-- @game with other services
const serverModule = require("@game/ServerScriptService/MyModule")
const starterModule = require("@game/StarterPlayer/StarterPlayerScripts/MyModule")
```

### Path Syntax Reference

| Syntax | Meaning | Example |
|--------|---------|---------|
| `@self/` | Relative to current script | `require("@self/utils")` |
| `./` | Same folder as current script | `require("./helper")` |
| `../` | Parent folder | `require("../shared")` |
| `@game/` | Absolute from game root | `require("@game/ReplicatedStorage/Module")` |

### Rules

1. Prefer `@self/`, `./`, `../` for relative requires within same package
2. Prefer `@game/` for cross-package requires
3. Never use `WaitForChild` in require paths unless module may not exist yet
4. Cache require results in `const` at module top

## Roblox Studio MCP tools — DO NOT USE
- Creator Store Search/Insertion
- Mesh/Material/Primitive Generation
- Store Image

## Testing
- MCP available → run playtest for every completed task; verify via console/output.
