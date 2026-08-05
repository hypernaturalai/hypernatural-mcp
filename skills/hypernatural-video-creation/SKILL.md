---
name: hypernatural-video-creation
description: Use when the user wants a video made or edited — "make me a video", a promo, ad, UGC clip, explainer, product or launch video, turning a script, blog post, or images into video — even when no tool is named, and whenever Hypernatural or its MCP server is mentioned.
---

# Creating videos with Hypernatural

Hypernatural (https://hypernatural.ai) turns prompts, scripts, and reference media into edited videos. Use MCP server `https://api.hypernatural.ai/mcp` (streamable HTTP; client handles OAuth). Tool descriptions define arguments; this skill defines workflow and UX.

## Core workflow

1. **Start from a shot list; its author decides the path.** Each `Shot 1: …` entry becomes exactly one shot.
   - **User supplied the shots:** preserve wording, weave in required `@Name`s as binding edits, then create immediately. Those `@Name` edits are the only change you make without approval; planner adds the rest.
   - **You write or expand any of the shots beyond those `@Name` edits** — from prose, a brief, a vague idea, or by filling in detail the user did not give: read [references/shot-writing.md](references/shot-writing.md), then give each shot a concrete Visual (subjects, exact `@Name`s) and Animation (motion, camera, quoted on-screen speech first). **Show draft and get user approval before any `create_*` call.** Explicit waiver ("just make it, I trust you") counts: show list, do not wait for reply.
2. **Set up references before composition.** Upload user images. Route by content: scene, background, mood, or style uses `create_asset` and `static_image_references`; consistent product or logo uses `create_reference_object`; on-screen person uses `create_character`. Poll each job with `get_job` to `complete` before using new `@Name`. Entity does not exist earlier; never use it in same message as its `create_*` call.
3. **Call `create_composition` once.** Poll `get_composition`, not `get_job`; obey computed `next_action`. For `wait`, wait `retry_after_seconds`, poll again, and send no Hypernatural message or unsolicited poll update. Answer direct status questions. For `answer_question`, ask user and forward only their answer; a labeled recommendation is allowed, but never answer for them. Send nothing else because unrelated messages cancel question. Relay `top_up_credits` / `open_in_app` / `report_failure` guidance, then stop.
4. **Hand off composition `url`, never bare id.** Rendering and export happen in Hypernatural app, not over MCP.

## UX rules

- Edit with plain language through `send_chat_message`: exact entity `@Name`s; shots by position or content ("shot 2", "the cafe shot").
- Quote durations from `get_composition`, not requested values; narration refits timing. Ask for pacing words ("quick cuts") unless exact timing matters.
- A `complete` job in `get_job` / `list_jobs` can still have per-file failures. Check `error_message` and `result.errors`; listed files are absent from `result.assets`.
- Say generation is *kicked off*, not *ready*, until polling confirms it.
- Check team library with `list_assets`, `list_characters`, `list_reference_objects` before creating duplicates.

## When things fail

- Follow recovery instructions in error text and any `guidance` field before improvising.
- After ambiguous `create_composition` failure (timeout, server error), call `list_compositions` **before** retrying. Composition may exist; blind retry creates duplicate.
- Never loop identical failures. On `report_failure`, relay guidance with composition `url`, then stop. User can continue in app.

## Common mistakes

| Mistake | Instead |
|---|---|
| `create_composition` straight from a vague ask | Draft the shot list, get approval |
| Using an `@Name` in the same turn as its `create_*` | Poll the job to `complete` first |
| A `static_image_references` name that is not in `prompt` | Every name there must appear as `@Name` in `prompt`, or the call is rejected |
| Answering a `next_action` question yourself | Forward the user's own answer |
| Messaging while `next_action` is `wait` | Wait `retry_after_seconds`, poll again |
| Uploading a product photo as a plain asset | Products/logos are reference objects; people are characters |
