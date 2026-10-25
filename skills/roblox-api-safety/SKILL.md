---
name: roblox-api-safety
description: Detect and prevent deprecated Roblox API usage in Luau code. Triggers on Roblox/Luau code requests, deprecated methods, API errors, or API validation. Scans for deprecated APIs and auto-corrects with modern replacements.
---

# Roblox API Safety

Scan all Luau code for deprecated APIs. Flag and replace before execution.

## Verification
Do not stop until you have verified your own work. Re-check generated code against this list before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase only for local variables, member values, functions. PascalCase for Roblox APIs. LOUD_SNAKE_CASE for constants (`const`).

## Luau syntax (current, valid)
These are supported in Roblox Studio 2026. Never flag them as invalid:
- `const NAME = value` — immutable binding. Contextual keyword: valid wherever `local` is valid. Reassignment causes error. Works with type annotations (`const X: number = 1`) and function declarations (`const function foo()`).
- `if cond then expr else expr` — if-then-else expression. Preferred over `a and b or c` ternary idiom.
- `` `text {expr}` `` — string interpolation (backtick literals).
- `//` — floor division operator.
- `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `^=`, `..=` — compound assignment operators.
- `continue` — loop continue statement (not a keyword; contextual).
- `for k, v in tbl do` — generalized iteration (no `pairs`/`ipairs` needed for tables).
- `type Alias = ...` — type aliases; `export type` to share across modules.
- `@native`, `@yields`, `@checkedCast` — function attributes.
- Hex/binary literals: `0xFF`, `0b1010`. Digit separators: `1_048_576`.
- `\x`, `\u{}`, `\z` string escapes.

`const` makes the binding immutable, not the value. A `const` table still allows mutation of contents. Use `table.freeze()` when content immutability is required.

### When to use const vs local
- Services (`game:GetService("HttpService")`, etc.): always `const`. Never reassigned.
- Instances created once and never replaced: `const`.
- Config values, lookup tables, module references: `const`.
- Loop counters, accumulators, state that changes: `local`.
- Wrong: `local httpService = game:GetService("HttpService")`
- Right: `const HttpService = game:GetService("HttpService")`

## Deprecated → Modern

### Methods
| Deprecated | Modern |
|------------|--------|
| camelCase methods | PascalCase equivalent |
| `getService()` | `GetService()` |
| `Humanoid:LoadAnimation()` | `Animator:LoadAnimation()` |
| `FindPartOnRay()`, `FindPartOnRayWithIgnoreList()`, `FindPartsInRegion3()` | `workspace:Raycast()` + `RaycastParams` |
| `UserHasBadge()` | `UserHasBadgeAsync()` |
| `makeJoints()` | Constraints |
| `Player:LoadCharacter()` | `Player:LoadCharacterAsync()` |
| `table.foreach()`, `table.foreachi()`, `table.getn()` | Generic `for` loops / `#t` |

### Classes
| Deprecated | Modern |
|------------|--------|
| `BodyVelocity` | `LinearVelocity` |
| `BodyPosition` | `AlignPosition` |
| `BodyGyro` | `AlignOrientation` |
| `BodyAngularVelocity` | `AngularVelocity` |
| `Hat` | `Accessory` |
| `HopperBin` | `Tool` |

### Properties
`BasePart.Velocity`, `BasePart.RotVelocity`, `Humanoid.Torso`, `Player.CharacterAppearance`, `Workspace.FilteringEnabled` — all deprecated.

## Migration patterns

**Raycast:**
```lua
const params = RaycastParams.new()
params.FilterDescendantsInstances = {character}
params.FilterType = Enum.RaycastFilterType.Exclude
const result = workspace:Raycast(origin, direction, params)
```

**BodyVelocity → LinearVelocity:**
```lua
const att = Instance.new("Attachment", part)
const lv = Instance.new("LinearVelocity")
lv.Attachment0 = att
lv.MaxForce = math.huge
lv.VectorVelocity = Vector3.new(0, 50, 0)
lv.RelativeTo = Enum.ActuatorRelativeTo.World
lv.Parent = part
```

**Animation:**
```lua
const animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)
const track = animator:LoadAnimation(animInstance)
```

## Error adaptation
On `"X is not a valid member of Y"`:
1. Check casing (camelCase to PascalCase).
2. Check if deprecated entirely, find modern replacement.
3. Provide corrected code.

On `"attempt to assign to const variable"`: the variable was declared `const`. Either remove `const` if reassignment is intended, or use a different variable name.

## Checklist
- All Roblox API methods PascalCase
- No BodyMover classes
- Raycast via `workspace:Raycast()` + `RaycastParams`
- Animations via `Animator:LoadAnimation()`
- No `Region3` (use `GetPartBoundsInBox()`)
- DataStore calls use async variants
- Character spawning uses `LoadCharacterAsync()`
- `const` used for immutable bindings (services, config, references that never reassign)
- Services use `const`, not `local`
- No `table.foreach`, `table.foreachi`, `table.getn`

If uncertain: https://create.roblox.com/docs/reference/engine
