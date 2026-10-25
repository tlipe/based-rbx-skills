<div align="center">

# Based Rbx Skills

**Agent skills for Roblox Studio development.** Professional-grade discipline for Luau syntax, API safety, architecture, spatial reasoning, and engineering practices across all AI-assisted code generation.

[![License: MPL-2.0](https://img.shields.io/badge/License-MPL--2.0-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-5-green.svg)](#skills)
[![Luau](https://img.shields.io/badge/Luau-Roblox%20Studio%202026-orange.svg)](https://create.roblox.com/docs/luau)

</div>

---

## Install

```bash
npx skills add tlipe/based-rbx-skills
```

Or via full URL:

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
| [`roblox-api-safety`](skills/roblox-api-safety/SKILL.md) | Any Roblox/Luau code request | Detects deprecated APIs, enforces modern binding rules, and auto-corrects with current replacements. |
| [`roblox-luau`](skills/roblox-luau/SKILL.md) | Auto-loaded on all Luau output | Binding discipline, naming conventions, type system usage, modern require syntax, string interpolation, compound assignment. |
| [`roblox-ddd`](skills/roblox-ddd/SKILL.md) | Architecture / module structure tasks | Domain-driven layout for Roblox: shared/server/client separation, controller/service lifecycle, index-as-data rule, direct `return { ... }` tables. |
| [`roblox-geometry`](skills/roblox-geometry/SKILL.md) | CFrame, Vector3, movement, orientation | Spatial reasoning contract: coordinate-space discipline, LookVector derivation, right-vector cross order, Q/E yaw signs, top-down camera exceptions. |
| [`roblox-best-practices`](skills/roblox-best-practices/SKILL.md) | Writing, reviewing, debugging any Studio code | Instance creation guards, networking/data-store abstraction, client/server boundary enforcement, validation rules, testing workflow. |

---

## Design principles

1. **Modern Luau only.** Current syntax, no legacy patterns. The language evolved; the skills follow it.
2. **Zero ritual code.** No defensive wrappers, no unnecessary freezes, no intermediate variables where a direct return suffices. Every line earns its place.
3. **Explicit over implicit.** Naming encodes intent. Boundaries are enforced at the module level. Immutability is declared, not assumed.
4. **Verify before reporting done.** Every skill mandates playtest + console check or structural re-read before completion. Untested code is unfinished code.

---

## About quality

These skills encode practices from professional Roblox developers: safe component usage, optimized architecture, strict API discipline, and verified workflows. They eliminate the most common AI mistakes — deprecated calls, sloppy bindings, ritual defenses, broken boundaries.

Output quality still depends on task complexity, model capability, and prompt clarity. Skills raise the floor. They do not promise a ceiling.

---

## Compatibility

- Roblox Studio 2026 (full Luau feature set)
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
    <img src="https://img.shields.io/badge/Star_this_Project-%E2%98%85-yellow?style=for-the-badge&logo=github" alt="Star this project" />
  </a>
</p>
