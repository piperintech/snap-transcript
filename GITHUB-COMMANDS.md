# GitHub Setup Commands — Copy & Paste

Use these commands to get everything on GitHub and live.

---

## 1. Create Repo (GitHub web)

```
Go to github.com → New repository
Name: snap-transcript-rag
Description: Self-learning bilingual claims translation tool
Visibility: Public (or Private if for SNAP only)
Click Create
```

---

## 2. Clone & Add Files (Terminal)

```bash
# Clone your new repo
git clone https://github.com/YOUR_USERNAME/snap-transcript-rag.git
cd snap-transcript-rag

# Copy the files you built
cp ~/Downloads/snap-transcript-rag.html .
cp ~/Downloads/snap-kb-manager.html .
cp ~/Downloads/snap-knowledge-base.json .
cp ~/Downloads/README.md .
cp ~/Downloads/.gitignore .
```

---

## 3. First Commit

```bash
# Stage all files
git add .

# Commit
git commit -m "Initial commit: SNAP Transcript RAG tool with KB Manager"

# Push to GitHub
git push origin main
```

---

## 4. Enable GitHub Pages (GitHub web)

```
Go to your repo → Settings → Pages
Under "Build and deployment":
  Source: Deploy from a branch
  Branch: main
  Folder: /(root)
Click Save
```

Wait ~1 minute, then:

```
Your site is live at: https://YOUR_USERNAME.github.io/snap-transcript-rag/
```

---

## 5. Verify Files Are Live

Open in browser:
- `https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-transcript-rag.html`
- `https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-kb-manager.html`

Should load and work immediately.

---

## 6. Day-to-Day: Add New Patterns

```bash
# You added patterns in KB Manager, downloaded JSON

# Stage the updated KB
git add snap-knowledge-base.json

# Commit with a message describing what changed
git commit -m "Add: border crossing incident pattern + FNOL examples"

# Push to GitHub (live in ~30 seconds)
git push origin main
```

---

## 7. (Optional) Sync KB From KB Manager Auto-Save

If you set up GitHub API token for auto-commits:

```bash
# The KB Manager will auto-commit updates
# You'll see new commits appear in your GitHub repo automatically
# No terminal commands needed
```

---

## Troubleshooting

### "fatal: not a git repository"

```bash
# Make sure you're in the right folder
cd snap-transcript-rag
git status  # Should show your files
```

### "Permission denied" on push

```bash
# Use personal access token instead of password
# Go to Settings → Developer settings → Personal access tokens (classic)
# Create token with 'repo' scope
# When prompted for password, paste the token

git remote set-url origin https://YOUR_TOKEN@github.com/YOUR_USERNAME/snap-transcript-rag.git
git push origin main
```

### Changes not showing online

```bash
# GitHub Pages needs ~1 min to deploy
# Hard refresh: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows/Linux)
# Clear cache if still not working:
# Settings → Developer Tools → Network → Disable cache → Reload
```

### KB not updating in tools

```bash
# Make sure snap-knowledge-base.json is committed
git log snap-knowledge-base.json  # See commit history

# If it's not showing, commit again
git add snap-knowledge-base.json
git commit -m "Update KB"
git push origin main

# Hard refresh tools in browser
```

---

## Quick Git Tips

```bash
# See what files changed
git status

# See your commit history
git log --oneline

# See changes in a file before committing
git diff snap-knowledge-base.json

# Undo last commit (if you mess up)
git reset --soft HEAD~1

# See what's on GitHub (fetch without changing local)
git fetch

# Check what branch you're on
git branch
```

---

## Share With Team

Once live, share the links:

**For adjusters:**
> Use this tool to translate and summarize claims:
> https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-transcript-rag.html

**For KB updates (just you or tech team):**
> Manage SNAP's knowledge base here:
> https://YOUR_USERNAME.github.io/snap-transcript-rag/snap-kb-manager.html

---

## Private GitHub Repo?

If SNAP wants the repo private:

1. Go to repo → Settings → General → scroll to Danger Zone
2. Click "Change repository visibility" → Private
3. Anyone accessing needs GitHub login + access
4. Pages still public (can restrict in repo settings)

---

That's it! You're live. 🚀
