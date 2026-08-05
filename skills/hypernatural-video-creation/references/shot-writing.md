# Writing shot lists

Prompt feeds planner. Each `Shot N: …` entry becomes exactly one shot; planner preserves count, order, and intent while adding generation detail. Give every shot a **Visual** (still frame) and **Animation** (movement).

**These rules govern shot lists you draft.** For user-supplied shot lists, preserve wording. Weave in required `@Name`s as binding edits; do NOT restructure into Visual/Animation form or expand. Planner does that.

## Visual — becomes a still image

- Specify concrete subjects, objects, and placement. Avoid abstract or mood words (vibrant, breathtaking, symbolic) and style/camera/format words (cinematic, teal grade, 35mm). Style lives elsewhere; describe things, not vibes.
- **Every shot must stand alone.** Image model sees one shot with no memory of others. Re-name and re-describe recurring people and props every time: "@Maria pours a latte", never "she pours" or "the same barista". Repetition is correct.
- Use exact library `@Name`s every time entities appear. Never invent an `@Name` missing from library.
- Treat logo as graphic, not object. Put it on packaging, sign, or end-card; never stand it in scene like product.
- Never prescribe scene text (a sign reading "OPEN", title cards, captions). Describe prop generically. MCP on-screen text supports only auto-generated narration captions, shown or hidden through `send_chat_message`. For exact words, use narration so captions render them, or direct user to Hypernatural app editor. "Say it on screen" means text, not dialogue; do not turn it into quoted Animation speech.

## Animation — motion for the clip

- Give each shot one clear motion with matching energy: decisive for action ("the camera whips in as she spins"), subtle for calm ("steam curls off the cup"). Avoid identical gentle drift and chaotic quiet scenes.
- **For on-screen speech, lead Animation with exact quoted line**: `"Always add salt," the chef says as he stirs.` Unquoted speech verbs ("the chef explains", "she talks energetically") produce silent mouth movement. Without a line, use silent action instead (gestures, nods, raises a glass).
- Put speed and time effects (slow-motion, time-lapse, freeze-frame) in Animation, never Visual; still images cannot show elapsed time.

## Structure

- Interleave quoted narration between shots when needed. Never repeat on-screen dialogue as voiceover.
- Ask for pacing words ("quick cuts", "let it linger"), not per-shot seconds. Narration refits durations. Quote durations from `get_composition`, not requested values.

## Worked example

User has library character `@Maria`, reference object `@Fresco Bag`, and wants short café ad.

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

Why it works: each shot re-names subjects and stands alone; exact `@Name`s mark every entity appearance; quoted speech leads Animation; silent shots still move; no on-screen text or style words; narration is interleaved, not duplicated; pacing uses words.
