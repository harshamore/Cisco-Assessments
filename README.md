# Cisco IQ — components, deployment and low-level build

An interactive, single-page reference for Cisco IQ. Pick a deployment mode and see which components you need, then work through a phased build sequence with prerequisites, click paths, config values, verification steps and dependency gates.

Three tabs:

- **Deployment** — a mode picker (SaaS / on-prem tethered / on-prem air-gapped) driving two views: *What you need* (a component mind map with required, recommended, optional and not-used status) and *Build sequence* (a numbered low-level design, 15–18 steps across 5–6 phases depending on mode).
- **Components** — 22 components, each with a plain-language summary, how it works, what it needs, which deployment modes it applies to, and the gotchas.
- **Reference** — mode comparison, entitlement tiers, data-source matrix, appliance sizing, ports, password policy, glossary, known issues, lineage and roadmap.

Built from Cisco documentation current to the August 2026 release. Sources are listed at the bottom of the Reference tab.

---

## What's in this repo

```
index.html    the entire site — HTML, CSS and JS in one file
README.md     this file
```

No build step, no dependencies, no framework. The only external request is to Google Fonts for IBM Plex. If your environment blocks that, the page still works and falls back to system fonts.

---

## Hosting it on GitHub Pages

Two routes. **Route A** needs nothing but a browser. **Route B** uses git, and is better if you expect to update this regularly.

### Route A — browser only

**1. Create a GitHub account**

Go to <https://github.com> and sign up if you don't already have one. A free account is enough; GitHub Pages is included.

**2. Create a repository**

Click the **+** in the top right → **New repository**.

| Field | Value |
| --- | --- |
| Repository name | `cisco-iq-guide` (or anything — this becomes part of your URL) |
| Visibility | **Public** (see the note on private repos below) |
| Add a README | Leave unticked — you're uploading your own |

Click **Create repository**.

**3. Upload the files**

On the empty repo page, click **uploading an existing file**. Drag `index.html` and `README.md` in, then click **Commit changes**.

The file *must* be named `index.html` and sit at the top level of the repo. That's the file GitHub Pages serves when someone visits your root URL.

**4. Turn on GitHub Pages**

In the repo, go to **Settings** (top nav) → **Pages** (left sidebar).

Under **Build and deployment**:

- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**

Click **Save**.

**5. Wait, then visit your URL**

First publish takes roughly one to three minutes. Refresh the Settings → Pages screen and your URL appears at the top:

```
https://<your-username>.github.io/cisco-iq-guide/
```

If you see a 404, wait another minute and hard-refresh. If it persists, check that the file is named exactly `index.html` and is at the repo root, not inside a folder.

**6. Updating it later**

Open `index.html` in the repo, click the pencil icon, edit, and commit. Or delete the file and re-upload a new version. Either way the site rebuilds automatically in about a minute. You may need a hard refresh (Ctrl+Shift+R, or Cmd+Shift+R on a Mac) to get past your browser cache.

---

### Route B — using git

Better if you'll iterate on this, since you get proper version history and can work locally.

**Prerequisites:** git installed (`git --version` to check; <https://git-scm.com/downloads> if not) and a GitHub account.

Create the empty repository on GitHub first, as in steps 1–2 above, but this time don't upload anything.

```bash
# from the folder containing index.html and README.md
git init
git add index.html README.md
git commit -m "Cisco IQ interactive guide"
git branch -M main
git remote add origin https://github.com/<your-username>/cisco-iq-guide.git
git push -u origin main
```

If you're pushing over HTTPS, GitHub will ask for a password. Your account password won't work — you need a **personal access token**. Generate one at **Settings → Developer settings → Personal access tokens → Tokens (classic)**, tick the `repo` scope, and paste the token where it asks for a password. Alternatively, install the [GitHub CLI](https://cli.github.com/) and run `gh auth login`, which handles authentication for you.

Then turn on Pages exactly as in step 4 above.

To publish a change afterwards:

```bash
git add index.html
git commit -m "Update sizing table"
git push
```

---

## Things worth knowing before you publish

**Public repos are genuinely public.** Anyone with the URL can read the page, and search engines will index it. Everything in this file came from Cisco's public documentation, so that's fine as it stands — but if you later add customer names, account specifics, internal pricing or anything under NDA, a public GitHub Pages site is the wrong place for it.

**Private repos need a paid plan.** GitHub Pages from a private repository requires GitHub Pro, Team or Enterprise. On a free account, making the repo private silently disables the site. If you need this internal-only, host it somewhere your organisation controls instead — SharePoint, an internal web server, or Confluence as an attachment.

**Stopping search engines indexing it.** If the repo must stay public but you'd rather it didn't turn up in search results, add a file called `robots.txt` at the repo root containing:

```
User-agent: *
Disallow: /
```

This is a request, not enforcement. It stops well-behaved crawlers; it does not make the page private.

**Custom domain.** Under Settings → Pages there's a Custom domain field. You'll need a CNAME record at your DNS provider pointing to `<your-username>.github.io`. Tick **Enforce HTTPS** once the certificate provisions, which takes up to 24 hours.

**Sharing offline.** The file is fully self-contained, so you can also just email `index.html` to someone. It opens in any browser with no server at all. Only the web fonts need a connection, and it degrades cleanly without them.

---

## Keeping it current

Cisco IQ ships monthly. Sizing figures, port lists, known issues and roadmap dates all move. Before using any specific number with a customer, check it against the current release notes and the linked guides at the bottom of the Reference tab.

The content most likely to go stale first:

- Known issues — these change every release
- Roadmap timings — FY27 items may land, slip or be dropped
- On-prem tethered details — the thinnest-documented mode, and partly inferred here
- Appliance sizing and validated GPU list — expect this to expand
