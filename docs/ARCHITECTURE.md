# Architecture

Lax is a memory pipeline with four stages. Each stage only reads the output of the
previous one, so they can be built, tested and improved independently.

```
 Capture ──► Summarize ──► Recognize ──► Learn
 (raw text)   (episodes)    (entities)    (Markdown pages)
     └────────────── SQLite ──────────────┘        └── ~/Lax/
```

## 1. Capture

- Every few seconds, read the focused app, window title and visible text through
  the macOS Accessibility API (`AXUIElement`).
- If an app exposes no accessible text, fall back to a screenshot of the window
  (ScreenCaptureKit) and on-device OCR (Apple Vision). The screenshot is discarded
  after OCR.
- Skip unchanged content (hash-based dedup) and anything on the exclude list
  (password managers, private browser windows, user-chosen apps).
- Store as `captures` rows in SQLite: timestamp, app, window title, URL (if browser), text.

## 2. Summarize

- Every ~15 minutes (and when idle), group recent captures into time blocks and
  ask the local LLM to write short **episodes**: what was done, in which app, with whom.
- Episodes are what the rest of the system reads. Raw captures can expire after
  a configurable retention period.

## 3. Recognize

- Extract **entities** (projects, people, organisations, topics) from new episodes
  using structured LLM output (JSON).
- Resolve them against known entities (name, aliases, embeddings) to avoid
  duplicates ("Q3 offert" vs "Q3 proposal").

## 4. Learn

- Each entity has a Markdown page in the memory folder, e.g.
  `~/Lax/projects/q3-proposal.md`, `~/Lax/people/anna.md`, plus `~/Lax/about-me.md`.
- Pages are updated incrementally: the LLM gets the current page plus new episodes
  and proposes an updated page.
- **User edits win.** If the user edits a page, the edit is treated as ground truth
  and the model is instructed not to contradict it.

## Models

| Task | Default | Notes |
|---|---|---|
| Summaries, extraction, page updates | 4–8B instruct model via Ollama | Must handle Swedish and English |
| Embeddings | Small local embedding model | For entity resolution and search |
| Audio (later) | Whisper (e.g. KB-Whisper for Swedish) | Following Handy's approach |

The model backend is behind a trait so Ollama, LM Studio or MLX can be swapped in.

## Privacy guarantees

- No outgoing network calls except to `localhost` model servers and explicit,
  user-initiated model downloads.
- No telemetry, no crash reporting to third parties.
- Pause button, per-app exclude list, and "forget this time range".
