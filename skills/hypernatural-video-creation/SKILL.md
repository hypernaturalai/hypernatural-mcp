---
name: hypernatural-video-creation
description: Use when the user wants a video made or edited — "make me a video", a promo, ad, UGC clip, explainer, product or launch video, turning a script, blog post, or images into video — even when no tool is named, and whenever Hypernatural or its MCP server is mentioned.
---

# Creating videos with Hypernatural

Hypernatural (https://hypernatural.ai) turns prompts, scripts, and reference media into edited videos. Drive it through the MCP server at `https://api.hypernatural.ai/mcp` (streamable HTTP, OAuth handled by your client — add the URL and sign in). The server's tool descriptions document every argument; this skill covers the workflow and UX rules that make results good.

## Core workflow

1. **Draft a shot list before creating anything.** Each `Shot 1: …` entry becomes exactly one shot. If the user gave shot-level detail, relabel their own wording — no approval needed. If you invented or materially expanded anything (turned prose into shots, added shots, style, taglines), **show the drafted shot list and get the user's approval first** — no `create_composition`, `create_character`, or `create_reference_object` until they approve. An explicit waiver ("just make it, I trust you") counts as approval — still show the list, but don't block on a reply. Give every shot a concrete Visual (subjects, exact `@Name`s) and Animation (motion, camera); quoted on-screen speech goes first in Animation. Read [references/shot-writing.md](references/shot-writing.md) for the shot-writing rules and a worked example before drafting.
2. **Set up references before the composition.** Upload the user's images, route each by what it *is*: scene, background, mood, or style → `create_asset` (pass as `static_image_references`); a product or logo that must look identical wherever it appears → `create_reference_object`; a person who appears on screen → `create_character`. Poll each creator's job with `get_job` to `complete` before using the new `@Name` — the entity does not exist until its job completes, so never use it in the same message as its `create_*` call.
3. **One `create_composition` call**, then poll `get_composition` (not `get_job`) and do what `next_action` says — it is computed, not advisory. `wait` means wait `retry_after_seconds` and poll again — no message to Hypernatural and no per-poll updates to the user (answering their direct "is it done?" is fine). `answer_question` means ask the user and forward *their* answer — you may offer a labeled recommendation, but never answer for them — and send nothing else until it's answered, since an unrelated message cancels the question. Relay `top_up_credits` / `open_in_app` / `report_failure` guidance and stop.
4. **Hand off with the composition `url`** (never a bare id). Rendering and export happen in the Hypernatural app, not over MCP.

## UX rules

- Editing is plain language to `send_chat_message`: exact `@Name`s for entities, shots by position or content ("shot 2", "the cafe shot").
- Quote the durations `get_composition` reports, not the ones you asked for — narration refits timing. Ask for pacing in words ("quick cuts") unless the user wants exact timing.
- A `complete` job (in `get_job` / `list_jobs` payloads) can still contain per-file failures — check `error_message` and `result.errors`; files listed there are absent from `result.assets`.
- Close turns honestly: generation is *kicked off*, not *ready*, until polling says so.
- Reuse the team library (`list_assets`, `list_characters`, `list_reference_objects`) before creating duplicates.

## When things fail

- A tool error starting "Invalid arguments" — fix the named field and retry once.
- `create_composition` rejects an `@Name` — the entity is misspelled or its creation job hasn't completed: check the `list_*` tools, create/poll what's missing, retry.
- `create_composition` fails ambiguously (timeout, server error) — call `list_compositions` **before** retrying; the composition may exist, and a blind retry creates a duplicate.
- A job's `error_message` mentions blocked or moderated content — don't resubmit verbatim; adjust the flagged content with the user, then retry once.
- Per-file failures in a `complete` job (`result.errors`) — re-upload that file via a fresh `get_image_upload_urls` and recreate; the missing entity never appears on its own.
- `report_failure` — relay the guidance with the composition `url` and stop. Never loop retries on the same failure; the user can continue in the app.

## Common mistakes

| Mistake | Instead |
|---|---|
| `create_composition` straight from a vague ask | Draft the shot list, get approval |
| Using an `@Name` in the same turn as its `create_*` | Poll the job to `complete` first |
| Answering a `next_action` question yourself | Forward the user's own answer |
| Messaging while `next_action` is `wait` | Wait `retry_after_seconds`, poll again |
| Uploading a product photo as a plain asset | Products/logos are reference objects; people are characters |
