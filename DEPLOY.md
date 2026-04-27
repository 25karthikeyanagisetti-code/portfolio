# Deploy your portfolio to GitHub Pages

End state: a public URL like `https://25karthikeyanagisetti-code.github.io/portfolio/`
(or, if you use the user-site repo trick below, just `https://25karthikeyanagisetti-code.github.io/`).

There are two ways. Pick one.

---

## Option A — Project site (URL: `<user>.github.io/portfolio/`)

This is the safer option. Repo can be named anything; Pages serves the `main` branch.

### 1. Open Terminal on your Mac, navigate to the portfolio folder

```bash
cd ~/Library/Application\ Support/Claude/local-agent-mode-sessions/5f6354c8-bccc-401b-9c1f-a2481d5e7284/7348542c-8bc8-4f61-be69-51e7690476a5/local_dc547d5a-b694-44a7-bb25-2aa8aa88d155/outputs/portfolio
```

(Or wherever the `portfolio/` folder lives. You can also drag the folder into Terminal after typing `cd ` — it auto-fills the path.)

### 2. Drop your latest resume PDF into the folder

The folder already contains `Karthikeya_Nagisetti_Resume.pdf` (yesterday's version). Replace it with your final resume so the "Download résumé" button hits the correct file:

```bash
# replace this path with where your final resume PDF lives:
cp ~/Downloads/Karthikeya__Nagisetti__Resume.pdf ./Karthikeya_Nagisetti_Resume.pdf
```

### 3. Initialize the repo and make the first commit

```bash
git init
git add .
git commit -m "Initial portfolio site"
git branch -M main
```

### 4. Create the empty repo on GitHub

Go to **https://github.com/new** and create a repo named **`portfolio`** (public, no README, no .gitignore — leave it empty so GitHub doesn't reject the push). Do not check "Initialize with README".

### 5. Push to GitHub

Replace `<user>` with your GitHub username (`25karthikeyanagisetti-code`):

```bash
git remote add origin https://github.com/25karthikeyanagisetti-code/portfolio.git
git push -u origin main
```

If git asks you to authenticate, use a Personal Access Token, not your password (GitHub disabled passwords for git). Make one at **https://github.com/settings/tokens** → "Generate new token (classic)" → check the `repo` scope → copy the token and paste it as the password when git prompts.

### 6. Turn on GitHub Pages

Go to your repo on github.com → **Settings** (top tab) → **Pages** (left sidebar).

Under **Build and deployment**:
- **Source:** Deploy from a branch
- **Branch:** `main` / `(root)`
- Click **Save**

Wait 30–90 seconds. Refresh the Pages settings page. You'll see a green box that says:

> Your site is live at `https://25karthikeyanagisetti-code.github.io/portfolio/`

That's your URL. Visit it. Done.

---

## Option B — User site (URL: `<user>.github.io`, no `/portfolio/` suffix)

This is cleaner if you want the portfolio to *be* your GitHub homepage — but you can only have one user site per account, and the repo name has to match exactly.

```bash
cd ~/.../portfolio
git init
git add .
git commit -m "Initial portfolio site"
git branch -M main
git remote add origin https://github.com/25karthikeyanagisetti-code/25karthikeyanagisetti-code.github.io.git
git push -u origin main
```

Then create the repo on GitHub with the **exact** name `25karthikeyanagisetti-code.github.io` (the username repeated, plus `.github.io`). Pages auto-publishes user repos with that name — you don't even need to flip the toggle in Settings. Live in 30–90 seconds at:

```
https://25karthikeyanagisetti-code.github.io
```

**Heads up:** if you go with Option B, your Snap-Time Agent demo at `25karthikeyanagisetti-code.github.io/snap-time-agent` is unaffected — it lives under the user-site as a project subpath. Both work side by side.

---

## After it's live — three quick wins

### 1. Custom domain (optional, ~$12/yr)

Buy `karthikeya-nagisetti.com` (or whatever you want) on Namecheap or Cloudflare. In your repo: **Settings → Pages → Custom domain** → enter the domain. Add the DNS records GitHub shows you on the registrar side. HTTPS is automatic. The resume and LinkedIn already reference `karthikeya-nagisetti.com` as a placeholder — once you own it and point it here, every link is live.

### 2. Re-deploying after edits

```bash
cd ~/.../portfolio
# make your edits to index.html, swap the resume PDF, etc.
git add .
git commit -m "Update copy"
git push
```

GitHub rebuilds in ~30 seconds. No further action needed.

### 3. Add the URL to your resume header

Once you have the live URL, swap `karthikeya-nagisetti.com` in your resume header for the real one (or for your custom domain once it propagates). This is the single highest-converting thing on the resume — every recruiter who clicks the URL lands on a 3D animated portfolio with your photo, your real metrics, and a clickable Snap-Time demo. It's the closest thing to "the resume that can't be ignored."

---

## Troubleshooting

**`fatal: remote origin already exists`** — you ran `git remote add` twice. Fix: `git remote remove origin` then re-add.

**Push rejected, remote contains work you do not have** — GitHub auto-created a README. Fix: `git pull origin main --rebase` then `git push -u origin main`.

**Pages site shows 404 after 5 minutes** — three usual causes: (a) Pages source is set to a different branch, (b) `index.html` isn't at the root of the repo, (c) you forgot to push. Verify `index.html` is visible in the repo root on github.com, then re-check **Settings → Pages**.

**Resume button shows "404 Not Found"** — the PDF filename in `index.html` doesn't match the actual file. The HTML expects `Karthikeya_Nagisetti_Resume.pdf` exactly. Either rename your PDF to that, or edit line ~234 in `index.html` to match your file name, commit, push.

**3D animation doesn't load** — three.js loads from `cdnjs.cloudflare.com`. If the page works locally but not on GitHub Pages, check the browser console (Cmd+Option+J on Chrome) — likely a content-security or mixed-content warning. The site uses HTTPS for the CDN already, so this rarely happens on GitHub Pages.
