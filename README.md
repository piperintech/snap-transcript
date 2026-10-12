# SNAP Transcript Translator (v1)

**The first version of SNAP's bilingual claims translator: paste a Spanish or English call transcript, get an English translation and an FNOL or claims-level summary, guided by a small knowledge base of SNAP claims vocabulary.**

➡️ **The current version is [snap-transcript-rag](https://github.com/piperintech/snap-transcript-rag)**, which moves the AI call behind a Netlify serverless function and adds the MT Entry format.

---

## What this version does

- **Translator** (`snap-transcript-rag.html`): translation plus an FNOL (ultra-compact) or Claims (detailed) summary, showing which knowledge-base entries were used
- **KB Manager** (`snap-kb-manager.html`): add abbreviations, incident patterns and examples; import/export the JSON
- **Knowledge base** (`snap-knowledge-base.json`): 15 claims abbreviations, 8 incident patterns (rear-end, parking lot, hit and run, intersection, lane change, backing, border, rental), 10+ raw → compact FNOL examples

## What I changed in v2, and why

| v1 (this repo) | v2 ([snap-transcript-rag](https://github.com/piperintech/snap-transcript-rag)) |
|---|---|
| Browser calls the Claude API directly | Calls go through a **Netlify function**, so the API key never reaches the browser |
| FNOL + Claims summaries | Adds **MT Entry**, the exact format the claims system expects |
| GitHub Pages | Netlify (static site + serverless) |

Moving the key server-side was the main reason for v2. A key used from browser JavaScript can be read by anyone who opens the page.

## Stack

HTML/CSS/JavaScript · Claude API · JSON knowledge base tracked in Git

> No real transcripts are stored in this repo.

---

Built by **Manuela Gonzalez Piper** · Product & AI · [GitHub](https://github.com/piperintech)
