# SNAP Transcript RAG — Self-Learning Claims Tool

A bilingual (English/Spanish) RAG-enhanced system for SNAP Insurance adjusters to **translate**, **summarize**, and **learn from** claim transcripts.

Built with Claude API + RAG + Knowledge Base management.

---

## What It Does

1. **Translator** (`snap-transcript-rag.html`)
   - Paste a call transcript (English or Spanish)
   - Get clean English translation
   - Generate FNOL (ultra-compact) or Claims-level (detailed) summaries
   - See what the knowledge base retrieved for that claim type

2. **KB Manager** (`snap-kb-manager.html`)
   - Add new SNAP abbreviations, incident patterns, FNOL examples
   - Download your KB as JSON for backup/Git
   - Import updated KBs to restore or merge changes
   - Track version history

3. **Knowledge Base** (`snap-knowledge-base.json`)
   - 15 core abbreviations (IV, CV, R/E, CLMT, etc.)
   - 8 incident type patterns (rear-end, parking lot, border, etc.)
   - 10+ FNOL examples (raw → compact transformation)
   - Manu's communication voice + SNAP professional tone
   - Integrations: MODOTECH, CLADIS, Lucy bot

---

## Quick Start

### Option A: Use Live (Recommended)

Go to:
- **Translator:** https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-transcript-rag.html
- **KB Manager:** https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-kb-manager.html

Tools load the latest KB from your repo automatically.

### Option B: Run Locally

```bash
git clone https://github.com/YOUR_USERNAME/snap-transcript-rag.git
cd snap-transcript-rag

# Open in browser
open snap-transcript-rag.html
open snap-kb-manager.html
```

---

## How to Self-Learn (Add New Patterns)

### Weekly Workflow

1. **Monday–Friday:** Adjusters use Translator for daily claims
   
2. **You spot a new pattern:**
   - E.g., motorcycle collisions, border checkpoints, rental vehicles
   
3. **Open KB Manager:**
   - Click **Incident Patterns** tab
   - Add the type, FNOL pattern, characteristics
   - Add 2–3 FNOL examples showing raw → compact
   
4. **Commit to GitHub:**
   ```bash
   # Commit the updated KB
   git add snap-knowledge-base.json
   git commit -m "Add: motorcycle collision pattern"
   git push origin main
   ```

5. **Next similar claim:**
   - RAG automatically retrieves your new pattern
   - Translator + Summarizer use updated context
   - No code changes needed

### Result: Living Documentation

Your KB becomes a **living record** of every claim type SNAP processes. Team can see full history in Git commits, review changes via pull requests, and roll back if needed.

---

## File Structure

```
snap-transcript-rag/
├── snap-transcript-rag.html       ← The Translator tool
├── snap-kb-manager.html            ← The KB Manager tool
├── snap-knowledge-base.json        ← Your knowledge base (Git-tracked)
├── README.md                        ← This file
└── .gitignore                       ← (Add .env if using GitHub token)
```

---

## Setup: GitHub Pages (One-Time)

1. Create a repo: `https://github.com/YOUR_USERNAME/snap-transcript-rag`
2. Push these 3 files to main branch
3. Go to **Settings** → **Pages**
4. Select **Deploy from branch** → **main** / **/(root)**
5. Save
6. Your site is live in ~1 minute

**That's it.** No build process, no API keys needed (Claude API handles auth).

---

## Configuration

### Claude API Key

The translator uses your Claude API key via a fetch call to `https://api.anthropic.com/v1/messages`.

**Security note:** Your API key is never exposed (calls from browser go direct to Anthropic). For production:
- Consider a backend proxy
- Use environment variables (not committed to Git)
- Rotate keys regularly

To set a custom API key, edit the fetch URL in `snap-transcript-rag.html` line ~400:

```javascript
fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        model: 'claude-sonnet-4-6',
        max_tokens: 2000,
        messages: [...]
    })
})
```

---

## Advanced: Auto-Sync KB to GitHub

Want to auto-commit KB updates without manual `git push`?

Use GitHub API + Personal Access Token:

```javascript
// In KB Manager script
async function syncToGitHub() {
    const token = 'your_personal_access_token';
    const owner = 'your_username';
    const repo = 'snap-transcript-rag';
    
    // Fetch current file SHA
    const getRes = await fetch(
        `https://api.github.com/repos/${owner}/${repo}/contents/snap-knowledge-base.json`,
        { headers: { 'Authorization': `token ${token}` } }
    );
    const file = await getRes.json();
    
    // Update the file
    await fetch(
        `https://api.github.com/repos/${owner}/${repo}/contents/snap-knowledge-base.json`,
        {
            method: 'PUT',
            headers: { 'Authorization': `token ${token}`, 'Content-Type': 'application/json' },
            body: JSON.stringify({
                message: `Auto-update KB: ${new Date().toLocaleString()}`,
                content: btoa(JSON.stringify(kb, null, 2)), // base64
                sha: file.sha
            })
        }
    );
}
```

See `GITHUB-SETUP.md` for full implementation.

---

## SNAP Terminology (Quick Reference)

| Abbr | Full | Usage |
|------|------|-------|
| IV | Insured Vehicle | SNAP's client's car |
| CV | Claimant Vehicle | The other party's car |
| R/E | Rear Ended | Hit from behind |
| CLMT | Claimant | Other party making claim |
| INSD | Insured | SNAP's policyholder |
| PSGR | Passenger | Non-driver in vehicle |
| PKG LOT | Parking Lot | Loss at parking area |
| APTS | Apartments | Loss at residential complex |
| UNINS | Uninsured | Other party has no insurance |
| SUBR | Subrogation | Pursue other at-fault party |

---

## Incident Patterns in KB

The knowledge base includes:
- Rear-End Collision
- Parking Lot Collision
- Hit and Run
- Intersection Collision
- Lane Change Collision
- Backing Up Collision
- Border/International Incident
- Rental Vehicle Incident

Each has:
- FNOL compacting pattern
- Key characteristics
- Common context (what makes it complex/simple)

---

## Integrations

### MODOTECH
FNOL format (ultra-compact, 1–3 sentences, ALL CAPS) feeds directly into SNAP's claims system for fast intake.

### Lucy Bot (FNOL Voice)
Future: This RAG system can improve Lucy's prompt generation for call transcription + auto-FNOL.

### CLADIS
Design system: Navy deep (#051c2b), Arsenal serif titles, Inter body text. All tools match the unified SNAP design language.

---

## Troubleshooting

### KB Not Loading?
- Check browser console (F12 → Console)
- Verify `snap-knowledge-base.json` is in repo root
- GitHub Pages may need ~1 min to deploy changes

### Translator Returns Error?
- Verify Claude API is reachable
- Check API key in code
- Ensure transcript is not empty

### Changes Not Showing?
- Hard refresh browser (Cmd+Shift+R or Ctrl+Shift+R)
- Clear browser cache
- Check that JSON is valid (paste in jsonlint.com)

---

## Contributing

Team members can:
1. Use the Translator daily
2. Flag new patterns in the KB Manager
3. Create a pull request with updated KB
4. Manu reviews + merges
5. Everyone gets the updated patterns immediately

---

## License

SNAP Insurance Service LLC — Internal Use Only

---

## Support

Questions? Contact Manu (Manuela Gonzalez) — Operations & Technology, SNAP Insurance Service LLC

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-08-12 | Initial release: Translator, KB Manager, 15 abbreviations, 8 incident patterns, 10 FNOL examples |

---

**Built with ❤️ for SNAP adjusters by Manu**
