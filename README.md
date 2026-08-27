# DanHallMN.com

Static site for Dan Hall's ministry. Markdown + Liquid, built natively by
GitHub Pages' Jekyll — no Actions, no npm, nothing to run locally.

## How Dan publishes

He goes to **https://danhallmn.com/admin/**, clicks *Sign in with GitHub*, and
gets a real editor. Writing a post means: type a title, pick a date and a
category, write the body, click **Publish**. Sveltia CMS commits the Markdown
to this repo; GitHub Pages rebuilds in about a minute.

Posts live in `_opinions/` as a Jekyll collection rather than `_posts`, so
filenames carry no date prefix. That keeps URLs short (`/opinions/the-title/`)
and makes the CMS's "View on Live Site" link resolve exactly.

He can do the same for gallery photos (drag and drop) and for the standing copy
on About, Truth Report, Pastors, Intercessors, Donate, and Contact.

He never sees YAML, a filename convention, or a git commit. If the CMS ever
breaks, the content is still plain Markdown in `_opinions/` — editable through
GitHub's web UI, or by you.

## Structure

```
_config.yml           site-wide values + the prelaunch switch
_layouts/             default, page, post
_includes/            head, header, footer, preview bar, card icons, form slots
_opinions/            opinion pieces (this is what the CMS writes)
_data/audiences.yml   the three signup cards on the home page
_data/gallery.yml     gallery photos (CMS-managed)
admin/                Sveltia CMS — the editor Dan uses
index.html            coming-soon placeholder, served at /
home.html             the real home page, parked at /preview/ until launch
about.md, truth-report.md, pastors.md, intercessors.md,
donate.md, contact.md, opinions.html, gallery.html
```

`site.prelaunch: true` in `_config.yml` currently does two things: puts
`noindex, nofollow` on every real page, and shows the red preview bar.

## Preview links for Dan

| Page | URL |
|---|---|
| Home | https://danhallmn.com/preview/ |
| Opinions | https://danhallmn.com/opinions/ |
| A post | https://danhallmn.com/opinions/sample-post-you-can-delete/ |
| Gallery | https://danhallmn.com/gallery/ |
| About | https://danhallmn.com/about/ |
| Editor | https://danhallmn.com/admin/ |

## Setup still to do

### 1. CMS authentication (Cloudflare Worker)

The editor needs a tiny OAuth relay so Dan can click *Sign in with GitHub*.
It's Sveltia's official Worker: https://github.com/sveltia/sveltia-cms-auth

1. Deploy it — use the repo's *Deploy to Cloudflare* button, or
   `wrangler deploy` after `wrangler login`. Note the URL:
   `https://sveltia-cms-auth.<subdomain>.workers.dev`
2. Register a GitHub OAuth App at https://github.com/settings/applications/new
   - **Application name:** `Dan Hall Ministry CMS`
   - **Homepage URL:** `https://danhallmn.com`
   - **Authorization callback URL:** `<worker-url>/callback`
3. In the Worker's **Settings → Variables**, set:
   - `GITHUB_CLIENT_ID` — from step 2
   - `GITHUB_CLIENT_SECRET` — from step 2, click **Encrypt**
   - `ALLOWED_DOMAINS` — `danhallmn.com`
4. Worker URL is wired into `admin/config.yml` → `backend.base_url`:
   `https://sveltia-cms-auth.matt-b65.workers.dev`

### 2. Give Dan access

Invite his GitHub account as a **collaborator** on this repo with Write access.
That's what lets him publish. Nothing else about his account matters.

### 3. DNS (Cloudflare)

Apex `danhallmn.com` → four A records, **grey cloud / DNS only**:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Plus `CNAME www → matt-ortiz.github.io`, also DNS only. Proxying (orange cloud)
blocks GitHub's cert issuance and causes a redirect loop with Enforce HTTPS.
Once the cert issues, turn on **Enforce HTTPS** in repo Settings → Pages.

## Still needed from Dan

- [ ] **Professional headshot** — the About page has a stand-in block
- [ ] 5–10 ministry / event photos for the gallery
- [ ] **Inline Constant Contact embed codes** for all three lists (see below)
- [ ] Copy for the Pastors and Intercessors pages
- [ ] 3–5 opinion pieces to seed the Opinions page
- [ ] Confirm what **"CPR events"** stands for — it appears on his donation page
      and now on ours, but readers won't know the acronym
- [ ] Design inspiration: 2–3 sites he likes

Bio, mission statement, donation details, phone, and Facebook are all in and
sourced from his IMF/Kindful page.

### Why the Truth Report signup is a button, not an embed

The link Dan supplied is a *hosted landing page*
(`lp.constantcontactpages.com/su/...`), not an embed code. It sits behind a
bot-check interstitial, which does not complete inside a cross-origin iframe —
tested, and it renders blank. So the site links out to it in a new tab instead.

To get a true inline form, Dan needs to generate a **sign-up form embed code**
in Constant Contact (a different feature from a landing page). Save it as
`_includes/cc-<list>.html` and set `forms_ready: true` in `_config.yml`.

### Contact

`/contact/` lists four ways to reach Dan — email, phone, Facebook, mail — all
driven from `_config.yml`. No contact form by design: a static site can't
process one, so a form would mean a third-party handler and a delivery path
that can fail silently. A `mailto:` link cannot.

### Updating the CMS version

`admin/index.html` pins Sveltia to a specific version on purpose. It ships
roughly 40 releases a month and is still pre-1.0, so an unpinned CDN load can
change the editor without warning. To update: bump the version in that file,
open `/admin/`, and confirm it still works before pushing.

### The Truth Report form is too long

The hosted form requires eight fields: email, first name, last name, phone,
street, city, state, and postal code. That is a lot to ask of someone who just
wants a weekly email, and it will cost signups. Worth asking Dan to make
everything except email and name optional in Constant Contact.

Find every placeholder:

```bash
grep -rn "Placeholder\|EXAMPLE.COM" --include="*.html" --include="*.md" --include="*.yml" .
```

### Constant Contact forms

When an embed code arrives, save it as `_includes/cc-<list>.html` — the list
names are `truth-report`, `pastors`, `intercessors`. Then set
`forms_ready: true` in `_config.yml`. Until then each page shows a labelled
placeholder box instead of a broken form.

## Launching

1. Fill in the real content and flip `forms_ready: true`.
2. In `_config.yml`, set `prelaunch: false`.
3. Replace the placeholder: delete `index.html`, then in `home.html` change
   `permalink: /preview/` to `permalink: /`.
4. Commit and push. Confirm **Enforce HTTPS** is on.

## Local preview

Optional — GitHub Pages builds this on push, so you don't need Ruby locally.
If you want it: `bundle exec jekyll serve` with the `github-pages` gem.

## Donations

Kindful campaign link (live, do not change) — set in `_config.yml` as
`kindful_url`:
https://internationalministerialfellowship-bloom.kindful.com/?campaign=1419518
