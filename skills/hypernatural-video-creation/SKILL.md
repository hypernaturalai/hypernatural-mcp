---
name: hypernatural-video-creation
description: Create and edit AI-generated videos with Hypernatural from any MCP client or via the public REST API.
---

# Creating videos with Hypernatural

Hypernatural (https://hypernatural.ai) turns prompts, scripts, and reference material into fully edited videos with scenes, voiceover, music, and captions.

## MCP server (recommended)

Streamable HTTP endpoint: `https://api.hypernatural.ai/mcp`

Authentication is OAuth 2.0 with PKCE and dynamic client registration. Discovery metadata lives at `https://api.hypernatural.ai/.well-known/oauth-protected-resource/mcp`; most MCP clients handle the sign-in flow automatically when you add the server URL.

Quick registration:

- Claude Code: `claude mcp add --transport http hypernatural https://api.hypernatural.ai/mcp`
- Codex: `codex mcp add hypernatural --url https://api.hypernatural.ai/mcp`
- Cursor / Claude / ChatGPT: add `https://api.hypernatural.ai/mcp` as a custom connector or MCP server URL.

## Typical workflow

1. Optionally upload reference media (`get_image_upload_urls` then `create_asset`) and create reusable subjects (`create_reference_object` for products/logos, `create_character` for people).
2. `create_composition` with a prompt describing the video; seed it with reference objects or characters for visual consistency.
3. Track generation with `list_jobs` / `get_job` until the composition is ready.
4. Inspect the result with `get_composition`; request edits in natural language with `send_chat_message` (for example "make the intro shorter" or "swap the background music").
5. Direct the user to open the composition in the Hypernatural editor to fine-tune and export the final video.

## Tools

- `create_composition` — start a new video from a prompt
- `get_composition` / `list_compositions` — inspect existing videos
- `send_chat_message` — natural-language edits to a composition
- `get_image_upload_urls` / `create_asset` / `list_assets` — bring your own media
- `create_reference_object` / `list_reference_objects` — reusable product/object references
- `create_character` / `list_characters` — reusable people/character references
- `list_jobs` / `get_job` — track asynchronous generation work

## Public REST API

- OpenAPI spec: `https://api.hypernatural.ai/api/v1/schema.json`
- Documentation: `https://api.hypernatural.ai/api/v1/docs/`
- Authentication: Bearer API tokens, created in account settings at `https://app.hypernatural.ai`.
