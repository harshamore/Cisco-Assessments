# Cisco reference guides

Two interactive, self-contained single-page references, linked by a shared top navigation.

| Page | File | Covers |
| --- | --- | --- |
| **Cisco IQ** | `index.html` | Components, deployment modes (SaaS / on-prem tethered / on-prem air-gapped), phased low-level build sequence, technical reference |
| **Identity Security** | `identity-security.html` | Assessment with Identity Intelligence and Astrix; enforcement with Duo, Active Directory Defense and Zero Trust for Agents |

Both share the same structure: pick an option at the top, then switch between *What you need* (a component mind map with required / recommended / optional / not-used status) and *Build sequence* (a numbered low-level design with prerequisites, click paths, config values, verification and dependency gates). A Components tab holds the deep dives, and a Reference tab holds the tables, glossary and sources.

Built from Cisco documentation current to August 2026. Sources are listed at the bottom of each page's Reference tab.

---

## What's in this repo

```
index.html               Cisco IQ guide — also the site's landing page
identity-security.html   Identity Security guide
README.md                this file
```

No build step, no dependencies, no framework. Each page is a single file containing its own HTML, CSS and JavaScript. The only external request is to Google Fonts for IBM Plex; if your environment blocks that, the pages still work and fall back to system fonts.

### How the navigation works

Both pages carry the same `.sitebar` block just inside `<div class="wrap">`:

```html
<div class="sitebar">
  <span class="sitemark">Cisco reference</span>
  <nav class="sitenav" aria-label="Guides">
    <a href="index.html" aria-current="page">Cisco IQ</a>
    <a href="identity-security.html">Identity Security</a>
  </nav>
</div>
```

The only difference between the two files is which link carries `aria-current="page"` — that attribute is what the CSS uses to fill the active tab. The links are relative, so they work whether the site sits at the domain root or under a repository path.

Note there are two levels of tabs, styled differently on purpose. The **site nav** at the very top switches between guides and scrolls away. The **segmented control** below the title switches between sections within a guide and sticks to the top as you scroll.

### Adding a third guide

1. Copy an existing page as a starting point so you inherit the CSS and structure.
2. Add a link for it to the `.sitebar` block in **every** page, including the new one.
3. Set `aria-current="page"` on the new page's own link, and only there.

There's no template system here, so the nav is duplicated per file. That's the trade-off for having no build step. Past four or five pages it's worth moving to Jekyll, which GitHub Pages supports natively with layouts and includes.

---

## Hosting it on GitHub Pages

Two routes. **Route A** needs nothing but a browser. **Route B** uses git, and is better if you expect to update this regularly.

### Route A — browser only

**1. Create a GitHub account**

Go to <https://github.com> and sign up if you don't already have one. A free account is enough; GitHub Pages is included.

**2. Create a repository**

Click the **+** in the top right, then **New repository**.

| Field | Value |
| --- | --- |
| Repository name | `cisco-iq-guide` (or anything — this becomes part of your URL) |
| Visibility | **Public** (see the note on private repos below) |
| Add a README | Leave unticked — you're uploading your own |

Click **Create repository**.

**3. Upload the files**

On the empty repo page, click **uploading an existing file**. Drag in `index.html`, `identity-security.html` and `README.md`, then click **Commit changes**.

`index.html` must be named exactly that and sit at the top level of the repo — it's what GitHub Pages serves when someone visits your root URL. The other pages can be named anything; their filename becomes their path.

**4. Turn on GitHub Pages**

In the repo, go to **Settings** in the top nav, then **Pages** in the left sidebar.

Under **Build and deployment**:

- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**

Click **Save**.

**5. Wait, then visit your URL**

First publish takes roughly one to three minutes. Refresh the Settings then Pages screen and your URL appears at the top:

```
https://<your-username>.github.io/cisco-iq-guide/                        Cisco IQ
https://<your-username>.github.io/cisco-iq-guide/identity-security.html  Identity Security
```

You shouldn't need the second URL in practice — the site nav gets you there.

If you see a 404, wait another minute and hard-refresh. If it persists, check that `index.html` is named exactly that and sits at the repo root, not inside a folder.

**6. Updating it later**

Open the file in the repo, click the pencil icon, edit, and commit. Or delete it and re-upload a new version. Either way the site rebuilds automatically in about a minute. You may need a hard refresh (Ctrl+Shift+R, or Cmd+Shift+R on a Mac) to get past your browser cache.

---

### Route B — using git

Better if you'll iterate on this, since you get proper version history and can work locally.

**Prerequisites:** git installed (`git --version` to check; <https://git-scm.com/downloads> if not) and a GitHub account.

Create the empty repository on GitHub first, as in steps 1 and 2 above, but this time don't upload anything.

```bash
# from the folder containing the html files
git init
git add index.html identity-security.html README.md
git commit -m "Cisco reference guides"
git branch -M main
git remote add origin https://github.com/<your-username>/cisco-iq-guide.git
git push -u origin main
```

If you're pushing over HTTPS, GitHub will ask for a password. Your account password won't work — you need a **personal access token**. Generate one at **Settings → Developer settings → Personal access tokens → Tokens (classic)**, tick the `repo` scope, and paste the token where it asks for a password. Alternatively, install the [GitHub CLI](https://cli.github.com/) and run `gh auth login`, which handles authentication for you.

Then turn on Pages exactly as in step 4 above.

To publish a change afterwards:

```bash
git add .
git commit -m "Update appliance sizing table"
git push
```

---

## Things worth knowing before you publish

**Public repos are genuinely public.** Anyone with the URL can read the pages, and search engines will index them. Everything here came from Cisco's public documentation and public announcements, so that's fine as it stands — but it stops being fine the moment you add customer names, account specifics, internal pricing or anything under NDA.

**Private repos need a paid plan.** GitHub Pages from a private repository requires GitHub Pro, Team or Enterprise. On a free account, making the repo private silently disables the site. If you need this internal-only, host it somewhere your organisation controls instead — SharePoint, an internal web server, or Confluence as an attachment.

**Stopping search engines indexing it.** If the repo must stay public but you'd rather it didn't turn up in search results, add a file called `robots.txt` at the repo root containing:

```
User-agent: *
Disallow: /
```

This is a request, not enforcement. It stops well-behaved crawlers; it does not make the pages private.

**Custom domain.** Under Settings then Pages there's a Custom domain field. You'll need a CNAME record at your DNS provider pointing to `<your-username>.github.io`. Tick **Enforce HTTPS** once the certificate provisions, which takes up to 24 hours.

**Sharing offline.** Each page is fully self-contained, so you can also email one to someone and it opens in any browser with no server at all. The only thing that breaks is the site nav link to the other page, unless you send both files together in the same folder.

---

## Keeping it current

**Cisco IQ** ships monthly. Sizing figures, port lists, known issues and roadmap dates all move.

**Identity Security** is moving faster still. The Astrix acquisition closed in June 2026 and the integration boundary between Astrix and Identity Intelligence is still shifting; the supported agent gateway list and the agentic policy model both changed more than once during 2026.

Before using any specific figure or capability claim with a customer, check it against the current documentation linked at the bottom of each page's Reference tab. Each page also carries a section flagging the parts most likely to be stale.
