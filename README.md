# Hypernatural Agent Skills

Agent skills for [Hypernatural](https://hypernatural.ai) — AI video creation from any MCP client or the public REST API.

This repository is also an installable OpenAI plugin that bundles the Hypernatural MCP connection with these skills.

## Skills

| Skill | Description |
|---|---|
| [`hypernatural`](skills/hypernatural/SKILL.md) | Create or edit a video from a prompt, script, product, or images using the Hypernatural MCP server. |

## Layout

Each skill lives at `skills/<name>/SKILL.md` with `name` and `description` frontmatter. OpenAI-specific presentation and MCP dependency metadata lives beside it in `agents/openai.yaml`.

`skills/` — including each skill's `agents/openai.yaml` — is auto-synced from `hypernatural/mcp_skills/` in the [app repo](https://github.com/hypernaturalai/hypernatural); edit it there, not here.
