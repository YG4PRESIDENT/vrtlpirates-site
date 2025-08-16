# VRTL Pirates — parked site

A tiny static site with an animated sailing ship.

## 1) Add the ship image
- Save the provided image as `assets/@vrtlPirates.png` (transparent background PNG recommended).

On macOS you can run this to pick and copy your file interactively:

```bash
osascript -e 'set f to choose file with prompt "Select @vrtlPirates.png"' \
  -e 'set p to POSIX path of f' \
  -e 'do shell script "cp \"" & p & "\" /Users/yahirgonzalez/vrtlpirate.com/assets/@vrtlPirates.png"'
```

## 2) Preview locally
```bash
cd /Users/yahirgonzalez/vrtlpirate.com
python3 -m http.server 8080 | cat
# Open http://localhost:8080
```

## 3) Publish with GitHub Pages (automated via gh CLI)
If you have GitHub CLI installed and logged in (`gh auth status`), run:

```bash
cd /Users/yahirgonzalez/vrtlpirate.com
if ! command -v gh >/dev/null; then echo "Install GitHub CLI first: https://cli.github.com"; exit 1; fi
if ! gh auth status -h github.com >/dev/null 2>&1; then echo "Run: gh auth login"; exit 1; fi
REPO="vrtlpirates-site"
GITURL="https://github.com/$(gh api user -q .login)/$REPO.git"
 git init && git add . && git commit -m "Initial parked site" && git branch -M main
 gh repo create "$REPO" --source=. --public --push --remote=origin --confirm
 gh api -X PUT repos/{owner}/$REPO/pages -f source.build_type=workflow 2>/dev/null || true
 # Fallback to branch deploy if needed:
 gh api -X PUT repos/{owner}/$REPO/pages -f source.branch=main -f source.path="/" || true
 gh api -X POST repos/{owner}/$REPO/pages/hosts -f host="vrtlpirates.com" || true
 echo "Visit repo Settings → Pages to confirm deployment."
```

If you prefer manual steps, follow the Git commands below and enable Pages (Branch: `main`, Path: `/`).

## 4) Manual Git steps (if not using gh)
```bash
cd /Users/yahirgonzalez/vrtlpirate.com
git init
git add .
git commit -m "Initial parked site"
# Create a repo on GitHub and replace the URL below
git remote add origin https://github.com/<your-username>/vrtlpirates-site.git
git branch -M main
git push -u origin main
```

## 5) Point DNS at GitHub Pages
At your domain registrar (DNS settings), create these records:

- A — `@` → 185.199.108.153
- A — `@` → 185.199.109.153
- A — `@` → 185.199.110.153
- A — `@` → 185.199.111.153
- CNAME — `www` → `<your-username>.github.io`

TTL: default. Changes usually propagate within minutes (can take up to 24h).

## 6) Test
- Visit `https://vrtlpirates.com` and `https://www.vrtlpirates.com` after DNS propagates.
- If the ship image isn’t visible, confirm `assets/@vrtlPirates.png` exists in the repo. 