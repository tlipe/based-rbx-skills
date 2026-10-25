---
name: api-safety
description: Detect and prevent deprecated Roblox API usage in Luau code. Triggers on Roblox/Luau code requests, deprecated methods, API errors, or API validation. Scans for deprecated APIs and auto-corrects with modern replacements.
---

# Roblox API Safety

Scan all Luau code for deprecated APIs. Flag and replace before execution.

## Verification
Do not stop until you have verified your own work. Re-check generated code against this list before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase only for local identifiers. No UPPER_CASE, no snake_case.

## Deprecated → Modern

### Methods
| Deprecated | Modern |
|------------|--------|
| camelCase methods | PascalCase equivalent |
| `getService()` | `GetService()` |
| `Humanoid:LoadAnimation()` | `Animator:LoadAnimation()` |
| `FindPartOnRay()`, `FindPartOnRayWithIgnoreList()`, `FindPartsInRegion3()` | `Raycast()` + `RaycastParams` |
| `UserHasBadge()` | `UserHasBadgeAsync()` |
| `makeJoints()` | Constraints |
| `Player:LoadCharacter()` | `Player:LoadCharacterAsync()` |

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
local params = RaycastParams.new()
params.FilterDescendantsInstances = {character}
params.FilterType = Enum.RaycastFilterType.Exclude
local result = workspace:Raycast(origin, direction, params)
if result then local hit, pos = result.Instance, result.Position end
```

**BodyVelocity → LinearVelocity:**
```lua
local att = Instance.new("Attachment", part)
local lv = Instance.new("LinearVelocity")
lv.Attachment0 = att
lv.MaxForce = math.huge
lv.VectorVelocity = Vector3.new(0, 50, 0)
lv.RelativeTo = Enum.ActuatorRelativeTo.World
lv.Parent = part
```

**Animation:**
```lua
local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)
local track = animator:LoadAnimation(animInstance)
```

## Error adaptation
On `"X is not a valid member of Y"`:
1. Check casing (camelCase → PascalCase).
2. Check if deprecated entirely → find modern replacement.
3. Provide corrected code.

## Checklist
- All methods PascalCase
- No BodyMover classes
- Raycast via `Raycast()` + `RaycastParams`
- Animations via `Animator:LoadAnimation()`
- No `Region3` (use `GetPartBoundsInBox()`)
- DataStore calls use async variants
- Character spawning uses `LoadCharacterAsync()`

If uncertain: https://create.roblox.com/docs/reference/engine
