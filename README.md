# DanHallMN.com

Static site for Dan Hall's ministry. No build step — plain HTML/CSS, hosted on GitHub Pages.

## Pages

| File | Purpose | Public? |
|---|---|---|
| `index.html` | Coming-soon placeholder. This is what visitors to danhallmn.com see today. | Yes |
| `preview.html` | Working draft of the real home page, for Dan's review. `noindex, nofollow`. | Unlisted |

Preview link to send Dan once deployed: `https://danhallmn.com/preview.html`

## Local preview

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/ and http://localhost:8000/preview.html

## Still needed from Dan

- [ ] Public contact email address — currently `CONTACT@EXAMPLE.COM` in `index.html` and `preview.html`
- [ ] Mission statement (2–3 sentences)
- [ ] Biography (500–800 words) + professional headshot
- [ ] 5–10 ministry / event photos
- [ ] Constant Contact embed codes for all three lists (Truth Report, Pastors, State Intercessors)
- [ ] 3–5 opinion pieces to seed the Opinions page
- [ ] Design inspiration: 2–3 sites he likes
- [ ] Social links, if any

Every placeholder in the HTML is marked with `[Placeholder ...]` in italics or a
`TODO` comment, so they're easy to find:

```bash
grep -rn "Placeholder\|TODO\|EXAMPLE.COM" *.html
```

## Deploy (GitHub Pages)

1. Push to `main` on the site repo.
2. Repo → Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Custom domain: `danhallmn.com` (the `CNAME` file in this repo sets it).
4. Wait for the DNS check to pass, then tick **Enforce HTTPS**.

`.nojekyll` is present so GitHub serves the files as-is without running Jekyll.

## DNS

At the registrar, for the apex domain `danhallmn.com` — four A records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

And a CNAME for `www` → `<github-username>.github.io`

## Donations

Kindful campaign link (live, do not change):
https://internationalministerialfellowship-bloom.kindful.com/?campaign=1419518
