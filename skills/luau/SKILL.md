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
- camelCase for local variables, member values, functions, params, table keys. PascalCase for types/classes and Roblox API references. LOUD_SNAKE_CASE for `const` bindings holding config or service references. Enum members (`Enum.*`) exempt.

## Binding discipline
- `const`: services, configs, module refs, singletons, anything never reassigned. Example: `const HttpService = game:GetService("HttpService")`, `const maxRetries = 3`.
- `local`: loop counters, accumulators, mutable state, anything reassigned.
- Never use `local` for a binding that is never reassigned. That is wasted mutability.
- `const` makes the binding immutable, not the value. A `const` table still allows content mutation. Use `table.freeze()` only when content immutability has a concrete threat.
- Works with type annotations: `const timeout: number = 30`.
- Works with function declarations: `const function addOne(x: number): number ... end`.
- Multi-assignment: `const a, b = 1, 2`.

## Style
- Max performance + minimal size + readability. Modular, safe, no redundancy. Early return.
- Reassess constantly: can it be reduced? Made more direct?
- No `pairs`/`ipairs` — use generalized iteration `for i, v in table do`.
- Module util structure: `return function(...main params) local self = {} function self:method(...) end return self end`.
- Prefer string interpolation `` `{var}` `` over concatenation.
- Prefer if-then-else expressions over `a and b or c` ternary idiom.
- Prefer compound assignment (`+=`, `..=`, etc.) over verbose reassignment.
- Prefer floor division (`//`) over `math.floor(a / b)`.
- Use `continue` for loop skip instead of nesting if blocks.
- No IIFE. No unnecessary spaces.

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
5. All requires are `const`. Never `local`.

## Type system
- Gradual typing by default. Add annotations where they prevent real errors, not as decoration.
- `type Alias = ...` for local aliases. `export type` for cross-module sharing.
- Function types: `(argType) -> returnType`.
- Optional args: `number?` shorthand for `number | nil`.
- Array tables: `{T}` shorthand.
- Never annotate varargs with type.

## Attributes
- `@native` — marks function as native callable.
- `@yields` — marks function that yields (async).
- `@checkedCast` — narrows type on cast failure.

## Roblox Studio MCP tools — DO NOT USE
- Creator Store Search/Insertion
- Mesh/Material/Primitive Generation
- Store Image

## Testing
- MCP available → run playtest for every completed task; verify via console/output.
