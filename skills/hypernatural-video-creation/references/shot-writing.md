# Writing shot lists

The prompt is planner input: each `Shot N: …` entry becomes exactly one shot, and the planner preserves count, order, and intent while expanding generation detail. These rules make that expansion land. Give every shot a **Visual** (what the still frame shows) and an **Animation** (how it moves).

## Visual — becomes a still image

- Concrete subjects, objects, and placement. No abstract or mood words (vibrant, breathtaking, symbolic) and no style/camera/format words (cinematic, teal grade, 35mm) — style is handled separately; describe things, not vibes.
- **Every shot must stand alone.** The image model sees one shot at a time with no memory of the others. Re-name and re-describe recurring people and props in every shot they appear in — "@Maria pours a latte", never "she pours" or "the same barista". Repetition across shots is correct.
- Use exact `@Name`s for library entities every time they visually appear. Never invent an `@Name` that isn't in the library.
- A logo entity is a graphic, not an object: put it on packaging, a sign, or an end-card — never standing in the scene like a product.
- Never prescribe written text in the scene (a sign reading "OPEN", title cards, captions). Describe the prop generically instead. On-screen text over MCP is limited to auto-generated captions of the narration (shown/hidden on request via `send_chat_message`) — if the user wants specific words on screen, put them in a narration line (captions render them) or direct them to the Hypernatural app editor. "Say it on screen" means text, not spoken dialogue — don't turn it into a quoted line in the Animation.

## Animation — motion for the clip

- One clear motion per shot, energy matched to the scene: decisive moves for action ("the camera whips in as she spins"), subtle motion for calm ("steam curls off the cup"). Don't flatten every shot into the same gentle drift, and don't crank a quiet scene to chaos.
- **If a character speaks on screen, lead the Animation with the exact quoted line**: `"Always add salt," the chef says as he stirs.` An unquoted speech verb ("the chef explains", "she talks energetically") produces silent mouth movement — if you have no line to give, use a silent action instead (gestures, nods, raises a glass).
- Speed and time effects (slow-motion, time-lapse, freeze-frame) live in the Animation, never in the Visual — a still can't show the passage of time.

## Structure

- Interleave voiceover as quoted narration lines between shots when the video needs it. Never repeat on-screen dialogue as voiceover.
- Ask for pacing in words ("quick cuts", "let it linger"), not per-shot seconds — durations are refit around narration. Quote the durations `get_composition` reports afterward, not the ones requested.

## Worked example

User has `@Maria` (character) and `@Fresco Bag` (reference object) in the library and asked for a short café ad.

```
Shot 1: Visual: @Maria steams milk behind a sunlit café counter, @Fresco Bag on the
shelf beside the espresso machine. Animation: "This is how mornings should start,"
she says, swirling the milk pitcher.

Narration: "Small-batch. Roasted this week."

Shot 2: Visual: A white ceramic cup with tulip latte art on a wooden counter,
@Fresco Bag standing behind it. Animation: Steam curls slowly off the cup.

Shot 3: Visual: @Maria hands a to-go cup across the counter to a customer in a
denim jacket. Animation: "See you tomorrow," she says with a nod as the customer
turns to leave.

Shot 4: Visual: @Fresco Bag centered on a rustic wooden shelf between two potted
coffee plants. Animation: The camera pushes in slowly toward the bag.

Narration: "Fresco Coffee — small batches, roasted weekly."

Quick cuts on shots 1–3, let shot 4 linger.
```

Why it works: every shot re-names its subjects and stands alone; both entities appear by exact `@Name` in every shot that shows them; spoken lines are quoted and lead their Animation; silent shots still have real motion; no on-screen text or style words; narration is interleaved, not duplicated from dialogue; pacing is in words.
