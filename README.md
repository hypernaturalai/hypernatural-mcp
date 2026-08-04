# Hypernatural Agent Skills

Agent skills for [Hypernatural](https://hypernatural.ai) — AI video creation from any MCP client or the public REST API.

Skills are discoverable at [`https://hypernatural.ai/.well-known/agent-skills/index.json`](https://hypernatural.ai/.well-known/agent-skills/index.json) (per the [Agent Skills Discovery RFC](https://github.com/cloudflare/agent-skills-discovery-rfc)).

## Skills

| Skill | Description |
|---|---|
| [`hypernatural-video-creation`](skills/hypernatural-video-creation/SKILL.md) | Create and edit AI-generated videos with Hypernatural from any MCP client or via the public REST API. |

## Layout

Each skill lives at `skills/<name>/SKILL.md` with `name` and `description` frontmatter.

This repo is the source of truth. Pushes to `main` open a sync pull request against the Hypernatural app repo via CI; the served copies (and the digests in the discovery index) update when that PR merges and deploys.
