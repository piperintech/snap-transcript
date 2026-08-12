# SNAP Transcript RAG Tools — GitHub + Self-Learning Setup

## Quick Start: Host on GitHub Pages

### 1. Create a GitHub Repository

```bash
# Create a new repo on github.com called: snap-transcript-rag
# (or any name you want)

# Clone it locally
git clone https://github.com/YOUR_USERNAME/snap-transcript-rag.git
cd snap-transcript-rag
```

### 2. Add Your Files

```bash
# Copy the three files into your repo root:
cp snap-transcript-rag.html .
cp snap-kb-manager.html .
cp snap-knowledge-base.json .
```

### 3. Enable GitHub Pages

1. Go to **Settings** → **Pages**
2. Under "Source," select **Deploy from a branch**
3. Choose **main** branch, **/root** folder
4. Click Save
5. Your site is live at: `https://YOUR_USERNAME.github.io/snap-transcript-rag/`

Done. The tools are now publicly hosted (or private if your repo is private).

---

## Self-Learning: Workflow

The KB Manager currently saves to **browser localStorage** (unsaved when you close the browser).

To make it **persistent + self-learning**, use this workflow:

### A. Local Workflow (Manual)

1. **Use KB Manager** to add new terms, patterns, FNOL examples
2. **Click "Download as JSON"** → saves `snap-knowledge-base.json`
3. **Commit to Git**:
   ```bash
   git add snap-knowledge-base.json
   git commit -m "Add new FNOL pattern: motorcycle collision"
   git push origin main
   ```
4. **Tools auto-reload** the latest KB on page load (from your repo)

✅ **Pros:** Simple, manual control, no complexity  
❌ **Cons:** Requires manual commits

---

### B. Semi-Automated: Sync KB to GitHub

Modify `snap-kb-manager.html` to auto-save to GitHub when you click "Download."

**Setup (one-time):**

1. Create a **GitHub Personal Access Token**:
   - Go to Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Create token with `repo` scope
   - Copy the token (you'll need it)

2. In your repo, add a `.env` file (never commit this):
   ```
   GITHUB_TOKEN=your_token_here
   GITHUB_REPO=YOUR_USERNAME/snap-transcript-rag
   ```

3. Then use this fetch in the KB Manager script to auto-commit:

```javascript
async function autoCommitKbToGitHub() {
    const token = 'YOUR_PERSONAL_ACCESS_TOKEN'; // or read from env
    const owner = 'YOUR_USERNAME';
    const repo = 'snap-transcript-rag';
    const branch = 'main';
    
    const kbContent = JSON.stringify(kb, null, 2);
    const message = `Auto-update KB: ${new Date().toLocaleString()}`;
    
    // Get current file SHA (needed for update)
    const getRes = await fetch(
        `https://api.github.com/repos/${owner}/${repo}/contents/snap-knowledge-base.json`,
        { headers: { 'Authorization': `token ${token}` } }
    );
    const file = await getRes.json();
    
    // Update the file
    const updateRes = await fetch(
        `https://api.github.com/repos/${owner}/${repo}/contents/snap-knowledge-base.json`,
        {
            method: 'PUT',
            headers: { 'Authorization': `token ${token}`, 'Content-Type': 'application/json' },
            body: JSON.stringify({
                message: message,
                content: btoa(kbContent), // base64 encode
                sha: file.sha,
                branch: branch
            })
        }
    );
    
    if (updateRes.ok) {
        showStatus('KB synced to GitHub', 'success');
    }
}
```

**Add a button to KB Manager:**

```html
<button class="primary" id="syncGitHubBtn">Sync to GitHub</button>

<script>
document.getElementById('syncGitHubBtn').addEventListener('click', autoCommitKbToGitHub);
</script>
```

✅ **Pros:** Auto-syncs to GitHub, versioned history  
❌ **Cons:** Requires token, slightly more setup

---

### C. Full Auto-Learn: GitHub Actions (Advanced)

If you want the **tools to automatically learn** from claim patterns without manual intervention:

1. **Set up a webhook** from Slack/Discord/email to trigger a GitHub Action
2. **Action runs Claude** to analyze new claims and update KB
3. **Commits the updated KB** automatically

This is overkill for now but scalable. Would need:
- GitHub Actions workflow file (`.github/workflows/auto-learn.yml`)
- Claude API integration
- Webhook receiver

---

## Recommended Setup for You

**Start with: A (Manual Commit)**

1. Build your KB in the manager
2. Download the JSON
3. Commit it to GitHub with a message like "Add: border crossing patterns"
4. The tools always pull the latest KB from your repo

This gives you:
- ✅ Full version history
- ✅ No token management
- ✅ Control over what gets saved
- ✅ Easy to roll back if needed
- ✅ Scales with your team (others can PR improvements)

---

## File Structure

Your GitHub repo should look like:

```
snap-transcript-rag/
├── snap-transcript-rag.html
├── snap-kb-manager.html
├── snap-knowledge-base.json
├── README.md
└── .gitignore (add .env, .DS_Store, etc.)
```

---

## URLs

Once live on GitHub Pages:

- **Translator:** `https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-transcript-rag.html`
- **KB Manager:** `https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-kb-manager.html`

Share these URLs with your adjusters and claim team.

---

## Self-Learning in Practice

**Typical workflow each week:**

1. **Monday morning:** Adjusters use Translator for daily claims
2. **Wednesday:** You spot a new pattern (e.g., "border checkpoints")
3. **Use KB Manager** → Add incident type + 2-3 FNOL examples
4. **Download JSON** → Commit to GitHub with message
5. **Thursday:** Next similar claim → RAG automatically retrieves the new pattern

Over time, your KB becomes a living document of **every claim type SNAP sees**.

---

## Next: Add Your Logo + Custom Branding

Want to add the SNAP logo to the tools? Update the top bars:

```html
<div class="top-bar">
    <div style="display: flex; align-items: center; gap: 12px;">
        <img src="path-to-logo.png" style="height: 30px;">
        <h1>SNAP Transcript Translator</h1>
    </div>
    <a href="snap-kb-manager.html" class="kb-link-btn">KB Manager</a>
</div>
```

---

Questions? Ask me about:
- Deploying to a private GitHub repo (add collaborators)
- Automating KB backups
- Hosting on your own server instead of Pages
- Integrating with Lucy bot for auto-updates
