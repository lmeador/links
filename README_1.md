# Elizabeth Layton Center — link pages

Two pages, one shared list of links:

- `index.html` — the **public** page. This is the Instagram bio link.
- `internal.html` — the **staff** page. Socials, careers referrals, crisis line.
- `links.json` — everything both pages display. **This is the only file you'll edit.**
- `favicon.png` / `favicon-dark.png` — the dogwood mark, shown as the browser tab icon.

Both pages read the same `links.json`, so changing a social URL or the crisis line in
one place updates both. Both logo versions are embedded inside each HTML file, so
there's no separate logo file to manage.

**The two URLs will be:**

| | |
|---|---|
| Public | `links-a1b.workers.dev/` |
| Staff | `links-a1b.workers.dev/internal.html` |

---

## Part 1 — One-time setup (about 20 minutes)

### 1. Put the files on GitHub

1. Go to **github.com** → sign in (a free account is fine).
2. Click **+** (top right) → **New repository**.
3. Name it `links`. Set it to **Public**. Click **Create repository**.
4. On the next screen click **uploading an existing file**.
5. Drag in `index.html`, `internal.html`, `links.json`, `favicon.png`,
   `favicon-dark.png`, and this `README.md`.
6. Click **Commit changes**.

### 2. Connect Cloudflare

1. Go to **dash.cloudflare.com** → sign up (free).
2. Left sidebar: **Compute (Workers)** → **Create** → **Import a repository**.
3. Authorize GitHub, pick the `links` repo.
4. Framework preset: **None**. Build command: leave **blank**.
5. Click **Create and deploy**.

You'll get a URL like `links-a1b.workers.dev`. That's the live page — put it in the
Instagram bio.

> Cloudflare Pages works too and sets up almost identically, but Cloudflare now
> tells new projects to start with Workers, so use Workers.

### 3. (Later) Custom domain

When you want `links.laytoncenter.org`: in Cloudflare open the Worker →
**Settings** → **Domains & Routes** → **Add custom domain**. Whoever manages the
laytoncenter.org DNS adds one CNAME record. Free, certificate is automatic.

---

## Part 2 — Adding a link (30 seconds)

1. Open the repo → click `links.json` → click the **pencil** icon.
2. Copy an existing block inside `"featured"` and paste it at the top.
3. Change the four values. Use today's date.

```json
{
  "title": "Open house — October 14",
  "description": "Tours, staff Q&A, coffee",
  "url": "https://www.laytoncenter.org/open-house",
  "date": "2026-10-01"
},
```

4. **Commit changes** → **Commit changes** again.
5. Wait ~15 seconds, refresh. Done.

**To remove a link:** delete its block, including the comma after it.

This works on your phone's browser — GitHub's mobile editor is usable, so you can
add the link while you're drafting the Instagram post.

### What happens automatically

- **Newest first.** Items sort by `date`, so you never have to reorder anything.
- **The NEW badge** appears on the single newest item, and only for 21 days. After
  that it disappears on its own — no stale "NEW" sitting there in December.
- **Dates display** as "Aug 28" so visitors can tell what's current.

### The one thing that will bite you

JSON is fussy about commas. Every block needs a comma after it **except the last one
in a list**. If the page goes blank after an edit, that's almost always why — check
the commas, or paste me the file and I'll fix it.

---

## How the page is built

**Crisis block, pinned at the top.** Layton Center's crisis line first (local, fastest
route to real resources), 988 second. Both are labeled 24/7 and confidential so a
visitor doesn't have to guess. Edit either one in the `"crisis"` section of
`links.json`; delete the whole `"crisis"` block to remove it.

**Recently shared** — the rotating link-in-bio items. Dated, newest first. Keep it to
3–5; prune old ones when you add new ones.

**Always here** — services and careers. No dates, because they don't change.

**Design notes.** Playfair Display for the serif voice, Source Sans 3 for everything
you tap. The double rules between sections are the same device as the logo. Blue is
interactive, plum marks crisis, green marks the rotating section, and yellow is used
once, on the NEW badge, so it means something.

Both pages follow the visitor's light/dark setting. Each carries both logo versions —
black artwork on light, your white artwork on dark — and swaps between them, rather
than inverting one file in CSS. Same for the favicon: `favicon.png` for light browser
chrome, `favicon-dark.png` for dark.

---

## The staff page (`internal.html`)

Same brand, different job. Staff open it to follow and share the socials, and to
send the careers link to someone they're recruiting. It's laid out wider than the
public page because staff are usually on a laptop.

**What's on it:**

- **Crisis line** at the top, both numbers, labeled so nobody has to guess which is
  which.
- **Follow and share** — each account with its handle, a Follow button, and a small
  copy button. The copy button is there because "share it" in practice means pasting
  the URL somewhere. If you'd rather not have it, tell me and I'll pull it.
- **Hiring referrals** — the Paylocity link plus a short ready-to-paste message.
  "Copy message + link" puts both on the clipboard at once.
- **Other relevant links** — edit the `"otherLinks"` list in `links.json` to add
  anything else staff need. `"url": "index.html"` points at the public page and opens
  in the same tab; a full `https://` address opens in a new one.

**Edit the careers blurb** in the `"internal"` section of `links.json`:

```json
"internal": {
  "careersUrl": "...",
  "careersNote": "Always current — this link never goes stale.",
  "referralBlurb": "I work at the Elizabeth Layton Center and I think..."
}
```

I wrote that blurb as a starting point — rewrite it in your voice.

### About "unlisted"

The page still carries `noindex, nofollow, noarchive` in its `<head>`, so Google won't
list it, even though the on-page notice is gone. Nothing links to it from the public
page either.

It is deliberately **not** in `robots.txt` — a `Disallow: /internal.html` line would
publish the exact path to anyone who reads that file.

None of this stops someone who has the URL. Nothing on the page is sensitive, so
that's fine — but if staff-only content ever lands there, Cloudflare Access adds a
free login gate for small teams.

---

## Cost

$0. Cloudflare's free tier allows 100,000 requests a day — far more than a bio link
will see. GitHub is free. The only thing you'd ever pay for is a domain, and you
already own laytoncenter.org.
