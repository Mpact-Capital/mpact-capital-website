# MPact Capital — mpactcap.com

Refreshed marketing site for MPact Capital, LLC. Single-file static site
(no build step) that doubles as the front door to the **MPact Asset
Management Platform** — the Next.js + Supabase app in
`C:\Users\token\mpact-fund-management` (a.k.a. "Lighthouse").

Brand colors, logo, and section copy/bios mirror the live site
(mpactcap.com /divisions, /team, /advisory-board, /news).

## Layout

```
mpact-capital-website/
├─ index.html                         ← the whole site, single file
├─ CNAME                              ← mpactcap.com (GitHub Pages)
├─ assets/
│   ├─ mpact-logo.svg                 ← real logo from MPC Media Files
│   ├─ mpact-logo-transparent.png     ← transparent-bg variant
│   ├─ mpact-logo-transparent-2.png   ← alt transparent variant
│   ├─ marcus-martin.jpg              ← Marcus's headshot
│   ├─ favicon.svg                    ← vertical bar mark
│   └─ og-image.svg                   ← 1200×630 social card
└─ README.md
```

## Headshots to add

Drop these JPGs into `assets/` and the site will pick them up automatically —
the initials avatars are placeholders styled to match the brand:

- `milena-kohlhofer.jpg`
- `amari-sherrill.jpg`
- `jim-kelligrew.jpg`
- `julie-bernard.jpg`
- `euclid-walker.jpg`

When you have a file, update the corresponding `<div class="person-photo" aria-hidden="true">INITIALS</div>`
in `index.html` to:

```html
<div class="person-photo"><img src="assets/milena-kohlhofer.jpg" alt="Milena Kohlhofer"/></div>
```

…and remove the matching `<div class="photo-note">Drop assets/… </div>` line below the bio.

## View locally

Open `index.html` directly in a browser, or:

```bash
npx serve .
# or
python -m http.server 8000
```

## Wire the Staff / Client login

Both login buttons hit the same `/login` — the fund-management app
routes by `user_type` after AAL2 (STAFF → `/dashboard`, CLIENT →
`/portal`). Buttons append `?role=staff|client` so login copy can vary
if desired.

**Swap the URL in one place** — `CONFIG.LOGIN_URL` near the bottom of
`index.html`:

```js
const CONFIG = {
  LOGIN_URL: 'http://localhost:3000/login',          // dev
  // LOGIN_URL: 'https://portal.mpactcap.com/login', // prod (suggested)
  // LOGIN_URL: 'https://mpact-fund-management.vercel.app/login',
};
```

Recommended: deploy `mpact-fund-management` to Vercel and point
`portal.mpactcap.com` (Namecheap CNAME) at it. The marketing site on
the apex `mpactcap.com` then deep-links into `portal.mpactcap.com/login`.

## Brand system (from the real MpactCapital.svg logo)

| Token            | Hex         | Used for                                |
| ---------------- | ----------- | --------------------------------------- |
| `--ink-900`      | `#0A0820`   | Footer / deepest darks                  |
| `--ink-800`      | `#150F33`   | Logo background — primary dark          |
| `--ink-700`      | `#221A4F`   | Card backgrounds on dark                |
| `--magenta`      | `#FF00C8`   | M wordmark / primary CTA                |
| `--magenta-700`  | `#C30099`   | Magenta hover                           |
| `--orange`       | `#FF7B1C`   | "Pact" wordmark / secondary CTA         |
| `--orange-400`   | `#FFA45C`   | Orange hover / accent on dark           |
| `--cyan`         | `#00C7FD`   | "Capital" wordmark / info & links       |
| `--cyan-400`     | `#5BDDFF`   | Cyan accent on dark                     |
| `--yellow`       | `#FFD900`   | Sparing accent (dot, gradient)          |
| `--red-pink`     | `#FF0A64`   | Sparing accent                          |
| `--blue`         | `#054FA8`   | Deep cool accent                        |
| `--cream-50`     | `#FFF8F2`   | Light section background                |
| `--cream-100`    | `#FBEFE2`   | Warm secondary light background         |

Typography: **Fraunces** (display serif) + **Inter** (UI/body), both via
Google Fonts.

## Deploy (GitHub Pages — mirrors the Foundation site setup)

Mpactfoundation.org uses this exact pattern.

1. Create repo `Mpact-Capital/mpact-capital-website` (public — required
   for free Pages).
2. Push:
   ```bash
   git remote add origin https://github.com/Mpact-Capital/mpact-capital-website.git
   git push -u origin main
   ```
3. GitHub → repo → Settings → Pages → Source: `main` / root.
4. Custom domain: `mpactcap.com` (CNAME file is already in the repo).
5. Namecheap DNS for `mpactcap.com`:
   - 4 × A records on `@` pointing at GitHub Pages IPs:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `www` CNAME → `mpact-capital.github.io`
6. Wait ~1–2 min; verify:
   ```bash
   "/c/Program Files/GitHub CLI/gh.exe" api \
     repos/Mpact-Capital/mpact-capital-website/pages/builds/latest --jq .status
   ```
7. Enforce HTTPS in Pages settings once the cert provisions.

## Source for the verbatim content

- Divisions section → mpactcap.com/divisions
- Team bios → mpactcap.com/team
- Advisory Board bios → mpactcap.com/advisory-board
- News items → mpactcap.com/news
- Logo + transparents → `G:\Shared drives\MPC Logo & Media Files\Logo & Media Files\`
- Marcus headshot → `…\Partner Bios & Pics\MM_bio_pic_MPC.jpg`
