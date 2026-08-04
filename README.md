# Hypernatural Agent Skills

Agent skills for [Hypernatural](https://hypernatural.ai) — AI video creation from any MCP client or the public REST API.

Skills are discoverable at [`https://hypernatural.ai/.well-known/agent-skills/index.json`](https://hypernatural.ai/.well-known/agent-skills/index.json) (per the [Agent Skills Discovery RFC](https://github.com/cloudflare/agent-skills-discovery-rfc)).

## Skills

| Skill | Description |
|---|---|
| [`hypernatural-video-creation`](skills/hypernatural-video-creation/SKILL.md) | Create or edit a video from a prompt, script, product, or images using the Hypernatural MCP server. |

## Layout

Each skill lives at `skills/<name>/SKILL.md` with `name` and `description` frontmatter.
