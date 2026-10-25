---
name: roblox-geometry
description: Roblox 3D geometry and spatial reasoning conventions. Auto-loaded; use for CFrame, Vector3, direction, movement, orientation, coordinate space tasks.
---

# Roblox Geometry & Spatial Reasoning

The `geometry` agent owns deep spatial analysis. This skill is the quick-reference for any code touching 3D space.

## Verification
Do not stop until you have verified your own work. Sanity-check directions at identity/yaw=0 before reporting DONE.

## Code rules (strict)
- Code is English-only: identifiers, strings, logs, error messages. Zero Portuguese in code.
- No comments unless explicitly requested.
- camelCase for local variables, member values, functions, params, table keys. PascalCase for types/classes and Roblox API references. LOUD_SNAKE_CASE for `const` bindings holding config or service references. Enum members (`Enum.*`) exempt.
- Services always `const`, never `local`. Module refs, configs, singletons: `const`. Mutable state, loop counters, accumulators: `local`.
- `const` for immutable bindings (configs, service refs, lane tables). No `local` for immutables.
- No `pairs`/`ipairs` — use generalized iteration `for i, v in table do`.
- Prefer string interpolation `` `{var}` `` over concatenation.
- Prefer compound assignment (`+=`, etc.) over verbose reassignment.

## Core principles (non-negotiable)
1. CFrame is source of truth — never rely on `Position` alone for movement.
2. -Z is forward only locally — `LookVector` = forward, changes with rotation. Never assume world -Z.
3. Distinguish coordinate spaces — global vs local. Mixing = undefined behavior.
4. Normalize all direction vectors — `.Unit` after subtraction.
5. Orientation explicitly controlled — movement without rotation = visual bugs.

## Coordinate system
X right, Y up, Z back. `LookVector` → forward (-Z relative to object).

```lua
-- NEVER: global movement, ignores orientation
part.Position += Vector3.new(0, 0, -10)
-- ALWAYS: respects object orientation
part.CFrame = part.CFrame * CFrame.new(0, 0, -10)
```

## Input/camera sign conventions
- Identity: right `(1,0,0)`, up `(0,1,0)`, look `(0,0,-1)`.
- Right vector: `look:Cross(up)`; never `up:Cross(look)`.
- Camera-relative ground right: `look:Cross(Vector3.yAxis)`; normalize if horizontal length is above epsilon.
- Positive yaw turns left; Q = positive yaw, E = negative yaw.
- EXCEPTION: Top-down cameras (pitch > 45° looking down) invert Q/E perception. Use Q = yaw-, E = yaw+ for top-down. The orbital rotation appears reversed on screen when LookVector points downward.
- MouseWheel scroll up is positive Z; zoom in subtracts from distance/FOV.
- Screen/viewport X increases right; GUI Y increases down.
- Top-down pitch near -90°: do not use `LookVector.XZ` as forward; derive yaw from `RightVector` or stored yaw.

## Canonical patterns
```lua
const originCF = CFrame.lookAt(origin.Position, target.Position)

const forward = part.CFrame.LookVector
const toTarget = (target.Position - part.Position).Unit
if forward:Dot(toTarget) < 0 then end

part.CFrame = CFrame.lookAt(part.Position, target.Position) * CFrame.Angles(0, math.rad(180), 0)

part.CFrame += part.CFrame.LookVector * speed * dt

const worldCF = part.CFrame * CFrame.new(offset)
const localPos = part.CFrame:PointToObjectSpace(worldPoint)

part.CFrame = part.CFrame:Lerp(CFrame.lookAt(part.Position, target.Position), 0.1)
```

## Lane-based movement
```lua
const lanes = {-6, 0, 6}
local function update(character, laneIndex, speed, dt)
	const hrp = character.HumanoidRootPart
	const forward = hrp.CFrame.LookVector
	const right = hrp.CFrame.RightVector
	const nextPos = hrp.Position + forward * speed * dt + right * lanes[laneIndex]
	hrp.CFrame = CFrame.lookAt(nextPos, nextPos + forward)
end
```

## Common failures
- Mixing Position and CFrame → snapping, jitter, drift.
- Using `up:Cross(look)` for right → inverted A/D strafe.
- Using positive yaw for right rotation → inverted Q/E.
- Adding MouseWheel Z to distance/FOV when scroll-up should zoom in → inverted zoom.
- Global vectors (`Vector3.new(1,0,0)`) ignore rotation.
- Not normalizing → inconsistent speed.
- Assuming forward is constant → it changes with rotation.
- `MoveTo` without orientation → NPC slides sideways.
- Using third-person Q/E signs in top-down camera → inverted rotation. Check pitch: if > 45°, swap Q/E yaw signs.

## Safe movement contract
Every movement system MUST: use CFrame as base, derive forward from LookVector, lateral from RightVector, normalize direction vectors, re-apply orientation every frame.

If geometry is ambiguous, STOP. Define forward axis, movement space (local/global), rotation behavior. Only then proceed.
