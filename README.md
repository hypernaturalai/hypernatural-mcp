# Hypernatural Agent Skills

Agent skills for [Hypernatural](https://hypernatural.ai) — AI video creation from any MCP client or the public REST API.

This repository is also an installable OpenAI plugin that bundles the Hypernatural MCP connection with these skills.

## Skills

| Skill | Description |
|---|---|
| [`hypernatural-video-creation`](skills/hypernatural-video-creation/SKILL.md) | Create or edit a video from a prompt, script, product, or images using the Hypernatural MCP server. |

## Layout

Each skill lives at `skills/<name>/SKILL.md` with `name` and `description` frontmatter. OpenAI-specific presentation and MCP dependency metadata lives beside it in `agents/openai.yaml`.

This repo is the source of truth.
