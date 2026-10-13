# Professional AI Video Workflow

A repeatable, 8-stage pipeline for producing polished AI videos (travel films, brand promos, social reels, explainers) using the Higgsfield toolset available in Claude.

```
1 Brief → 2 Script & Shot List → 3 Look & Characters → 4 Keyframes
   → 5 Animate Shots → 6 Audio → 7 Edit & Finish → 8 QA & Deliver
```

The core rule of professional AI video: **never generate video straight from a text prompt for hero shots.** Lock the look in still images first (cheap, fast to iterate), then animate approved stills. This is what gives consistency between shots.

---

## Stage 1 — Creative Brief (15 min)

Fill in `templates/brief.md`. Decide before spending any credits:

| Decision | Options |
|---|---|
| Format | 16:9 (YouTube/web), 9:16 (Reels/TikTok/Shorts), 1:1, 21:9 (cinematic) |
| Length | 15 s / 30 s / 60 s / 2–3 min |
| Style | Photoreal cinematic · documentary · stylized/animated · UGC creator |
| Voice | On-camera presenter · voiceover narrator · music-only |
| Deliverables | Master + cutdowns (e.g. 60 s 16:9 master, 15 s 9:16 reel, thumbnail) |

## Stage 2 — Script & Shot List (30 min)

1. Write the script in beats: **Hook (0–3 s) → Context → 3–5 story beats → Payoff → CTA.**
2. Break each beat into shots in `templates/shot-list.csv`. Rule of thumb: **one shot = 3–6 s**, so a 30 s video ≈ 6–9 shots.
3. For every shot specify: framing (wide/medium/close), camera move (dolly in, orbit, crane up, handheld, static), subject action, lighting/time of day, and the VO line it covers.

## Stage 3 — Look Development & Consistency Assets

Lock these once and reuse them in every shot:

- **Style frame**: 1–2 reference images defining palette, lens, grade (e.g. "35 mm, golden hour, warm teal-orange grade, soft film grain").
- **Characters**: a character sheet for any recurring person (Higgsfield `character-sheet` workflow, or `ai_influencer_prepare/generate` for a consistent presenter). Store as reference elements.
- **Products/locations**: clean reference photos of anything that must look identical across shots.
- **Prompt suffix**: a fixed "style block" appended to every prompt (see `templates/prompt-templates.md`).

## Stage 4 — Keyframes (stills)

Generate the first frame of each shot with `generate_image` (use `generate_image_batch` for the whole shot list at once), always passing your style frame / character references.

- Make 2–4 variations per shot, pick the best, iterate only on rejects.
- Fix problems here (hands, text, faces, composition) — it is 10× cheaper than fixing video.
- Upscale approved keyframes with `upscale_image` before animating.
- Optional: generate an **end frame** too for shots with a precise transition.

## Stage 5 — Animate Shots (image → video)

Feed each approved keyframe as `start_image` (and optionally `end_image`) to `generate_video`. Use `generate_video_batch` for the whole list.

| Need | Recommended model |
|---|---|
| Hero cinematic shots, full camera/lens/lighting control | **Cinema Studio 4.0** (`cinematic_studio_video_4_0`) or Cinema Studio 3.0 (up to 4K) |
| Consistent character/product across many shots | **Seedance 2.0 / 2.5** (image + video + audio references, up to 30 s on 2.5) |
| Natural facial emotion & physics | Minimax Hailuo 2.3 |
| Longer clips with native synced audio | FLUX 3 Video (5–20 s) |
| Cheap drafts / blocking | Seedance 2.0 Mini, Grok Video 1.5 Lite, or Seedance 2.5 `draft: true` (480p, finalize later at 1080p) |
| One-click product/UGC ads | Marketing Studio, or the `ugc-video` workflow |

Tips:
- **Draft at 480p/720p first**, approve motion, then re-render finals at 1080p/4K.
- One camera move + one subject action per shot. Over-described motion = warped output.
- Generate 2 takes of each hero shot; pick the best.
- Extend a good shot instead of regenerating: Seedance 2.5 / Cinema Studio 4.0 `video_extension` mode.
- Fix a nearly-good shot with `video_edit` mode or FLUX 3 Video Edit rather than rerolling.

## Stage 6 — Audio

- **Voiceover**: `generate_audio` with a chosen voice (`list_voices`), or clone your own voice with `create_voice`. Generate VO *before* the final edit so shots are cut to the narration.
- **Presenter lip-sync**: `sync_so` (Sync Lipsync 3) to sync a generated presenter shot to the VO track.
- **Music & SFX**: `generate_audio` for score and ambience; or let models with native audio (Seedance, FLUX 3, Cinema Studio) generate in-shot sound.
- **Localization**: `dubbing` to translate the finished video into other languages.

## Stage 7 — Edit & Finish

- Assemble clips, music, and burned-in captions with the Higgsfield `video-montage` workflow, or `video-editing` (Higgsedit) for titles, lower thirds and motion graphics. (Or export clips to Premiere / DaVinci Resolve / CapCut.)
- Pacing: cut on motion, keep most shots 2–4 s in the final edit, put the strongest visual in the first 2 seconds.
- Finishing:
  - `upscale_video` / Topaz (1080p → 4K, frame interpolation for smooth 60 fps)
  - Topaz Hyperion for SDR → HDR
  - `reframe` to create 9:16 / 1:1 cutdowns from the 16:9 master
- Captions: always burn in for social (most viewers watch muted).
- Thumbnail/cover: `thumbnail-generation` workflow.

## Stage 8 — QA & Delivery

Run `templates/qa-checklist.md` on every export. Optionally score social cuts with `virality_predictor` (hook strength, retention risk) and re-cut the opening if it scores weak.

---

## Typical budget & time (30 s, 8-shot video)

| Stage | Time | Credit strategy |
|---|---|---|
| Brief + script + shot list | 45 min | free |
| Keyframes (8 shots × 3 variants) | 30 min | low — images are cheap |
| Video drafts (480p/720p) | 30 min | medium |
| Final renders (1080p/4K, best takes only) | 30 min | highest — only approved shots |
| Audio + edit + finish | 60 min | low |

Check balance before final renders with `balance`.

## Using this with Claude

Just say, for example:

> "Run the AI video workflow for a 30 s 9:16 reel about a sunrise hike in Cappadocia, cinematic style, female voiceover."

Claude will fill the brief, write the script and shot list, generate keyframes for your approval, then animate, voice, edit and upscale — pausing for approval before each credit-heavy stage.
