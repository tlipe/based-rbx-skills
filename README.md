<div align="center">

# Based Rbx Skills

**Agent skills for Roblox Studio development.** Enforce Luau modern syntax, API safety, DDD architecture, spatial reasoning, and engineering best practices across all AI-assisted code generation.

[![License: MPL-2.0](https://img.shields.io/badge/License-MPL--2.0-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-5-green.svg)](#skills)
[![Luau](https://img.shields.io/badge/Luau-Roblox%20Studio%202026-orange.svg)](https://create.roblox.com/docs/luau)

</div>

---

## Install

```bash
npx skills add tlipe/based-rbx-skills
```

Or method 2:

```bash
npx skills add https://github.com/tlipe/based-rbx-skills
```

Or manually clone into your agent's skill directory:

```bash
git clone https://github.com/tlipe/based-rbx-skills.git .agents/skills/based-rbx-skills
```

---

## Skills

| Skill | Trigger | Purpose |
|-------|---------|---------|
| [`api-safety`](skills/api-safety/SKILL.md) | Any Roblox/Luau code request | Detects deprecated APIs, enforces `const` bindings, modern Luau syntax, and auto-corrects with current replacements. |
| [`luau`](skills/luau/SKILL.md) | Auto-loaded on all Luau output | Binding discipline (`const` vs `local`), naming conventions, type system usage, modern require syntax, string interpolation, compound assignment. |
| [`ddd`](skills/ddd/SKILL.md) | Architecture / module structure tasks | Domain-driven layout for Roblox: shared/server/client separation, controller/service lifecycle, index-as-data rule, direct `return { ... }` tables. |
| [`geometry`](skills/geometry/SKILL.md) | CFrame, Vector3, movement, orientation | Spatial reasoning contract: local vs global space, LookVector derivation, right-vector cross order, Q/E yaw signs, top-down camera exceptions. |
| [`studio-best-practices`](skills/studio-best-practices/SKILL.md) | Writing, reviewing, debugging any Studio code | Instance creation guards, networking/data-store abstraction, client/server boundary enforcement, validation rules, testing workflow. |

---

## Design principles

1. **Modern Luau only.** `const`, if-expressions, generalized iteration, string interpolation, compound assignment, floor division. No legacy patterns.
2. **Zero ritual code.** No defensive wrappers, no unnecessary freezes, no intermediate variables where a direct return suffices.
3. **Explicit over implicit.** Services are `const`. Mutable state is `local`. Naming encodes intent. Boundaries are enforced at the module level.
4. **Verify before reporting done.** Every skill mandates playtest + console check or structural re-read before completion.

---

## About quality

These skills encode practices from professional Roblox developers: safe component usage, optimized architecture, strict API discipline, and verified workflows. They reduce common AI mistakes (deprecated APIs, wrong binding choices, ritual code, broken client/server boundaries).

Actual output quality still depends on the task complexity, the model used, and how well the prompt describes intent. Skills set the floor; they do not guarantee a ceiling.

## Compatibility

- Roblox Studio 2026 (Luau with `const` bindings support)
- Works with OpenCode, Cursor, Windsurf, Claude Code, and any agent that loads SKILL.md files
- Designed for Wails v3 desktop apps and standard Roblox projects alike

---

## Contributing

Pull requests welcome. Each skill lives in `skills/<name>/SKILL.md`. Keep entries concise, imperative, and free of commentary unless explicitly requested.

---

## Related skills

Other community projects targeting Roblox Studio agent workflows:

| Repository | Focus |
|------------|-------|
| [EL4CTEO/roblox-skills](https://github.com/EL4CTEO/roblox-skills) | General Roblox development skills |
| [TabooHarmony/roblox-brain](https://github.com/TabooHarmony/roblox-brain) | Knowledge base and reasoning aids |
| [MSayib/roblox-dev-skill](https://github.com/MSayib/roblox-dev-skill) | Development workflow skills |
| [justOzy/Roblox-Skills](https://github.com/justOzy/Roblox-Skills) | Curated skill collection |
| [flatbedj/roblox-skills](https://github.com/flatbedj/roblox-skills) | Community skill pack |
| [SyphoDev-yt/roblox-ai-skills](https://github.com/SyphoDev-yt/roblox-ai-skills) | AI-assisted Roblox coding |
| [AshExplained/roblox-skills](https://github.com/AshExplained/roblox-skills) | Tutorial-driven skills |
| [m4bwav/roblox-agent](https://github.com/m4bwav/roblox-agent) | Agent framework integration |
| [Roblox/roblox-studio-agent-plugin](https://github.com/Roblox/roblox-studio-agent-plugin) | Official Roblox agent plugin |

Install any with `npx skills add <owner>/<repo>`.

---

<div align="center">

Built for agents that write Roblox code correctly the first time.

</div>

---

<p align="center">
  If this project is useful to you, consider giving it a star.
  <br />
  <a href="https://github.com/tlipe/based-rbx-skills/stargazers">
    <img src="https://img.shields.io/badge/Star_this_Project-★-yellow?style=for-the-badge&logo=github" alt="Star this project" />
  </a>
</p>
