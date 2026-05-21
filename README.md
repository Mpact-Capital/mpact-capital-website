# MPact Capital — mpactcap.com

Production marketing site for MPact Capital, LLC. Single-file static
site (no build step) that doubles as the front door to the **MPact
Asset Management Platform** — the Next.js + Supabase app in
`C:\Users\token\mpact-fund-management` (a.k.a. "Lighthouse").

- **Live URL (after DNS):** https://mpactcap.com/
- **GitHub Pages URL:** https://mpact-capital.github.io/mpact-capital-website/
- **Repo:** [Mpact-Capital/mpact-capital-website](https://github.com/Mpact-Capital/mpact-capital-website)

## Layout

```
mpact-capital-website/
├─ index.html              ← the whole site, single file
├─ privacy.html            ← Privacy Policy stub
├─ terms.html              ← Terms & Conditions stub
├─ sitemap.xml             ← search-engine sitemap
├─ robots.txt              ← crawler config
├─ CNAME                   ← mpactcap.com (GitHub Pages custom domain)
├─ assets/
│   ├─ mpact-logo.svg          ← real stacked logo
│   ├─ mpact-mark.svg          ← bar-mark only (favicon + topbar)
│   ├─ favicon.svg
│   ├─ og-image.svg
│   ├─ marcus-martin.jpg / milena-kohlhofer.png / amari-sherrill.png
│   ├─ jim-kelligrew.jpg / julie-bernard.png / euclid-walker.jpg
│   ├─ partner-avalanche.svg / partner-homium.svg / partner-hedera.svg
│   └─ mpact-logo-transparent[-2].png
└─ README.md
```

## Going live — DNS at Namecheap

The repo, `CNAME` file, and GitHub Pages custom domain are already
configured to serve `mpactcap.com`. The only remaining step is
pointing the domain's DNS at GitHub Pages.

### Steps in Namecheap (BasicDNS)

1. **Domain List → Manage → Advanced DNS** for `mpactcap.com`.
2. **Delete** any existing records that point at Google Sites
   (typically a CNAME on `@` and `www` to `ghs.googlehosted.com` or
   similar).
3. **Add 4 A records on `@`** (the apex), each with TTL Automatic:

   | Type | Host | Value             |
   | ---- | ---- | ----------------- |
   | A    | @    | 185.199.108.153   |
   | A    | @    | 185.199.109.153   |
   | A    | @    | 185.199.110.153   |
   | A    | @    | 185.199.111.153   |

4. **Add `www` CNAME**:

   | Type  | Host | Value                          |
   | ----- | ---- | ------------------------------ |
   | CNAME | www  | mpact-capital.github.io.       |

   (Trailing dot optional — Namecheap accepts either.)

5. Save changes. Propagation: 5 minutes to a few hours.
6. After DNS resolves, GitHub Pages will issue a free Let's Encrypt
   cert. Then enable HTTPS-only:

   ```bash
   "/c/Program Files/GitHub CLI/gh.exe" api -X PUT \
     repos/Mpact-Capital/mpact-capital-website/pages \
     -f cname=mpactcap.com -F https_enforced=true
   ```

7. Verify build:

   ```bash
   "/c/Program Files/GitHub CLI/gh.exe" api \
     repos/Mpact-Capital/mpact-capital-website/pages/builds/latest --jq .status
   # want "built"
   ```

### Expected downtime

There will be a few-minute window during DNS flip where the apex
points at GitHub Pages but the cert hasn't issued. The Google Sites
version becomes unreachable as soon as DNS propagates. Plan the cut
during off-hours if that matters.

## Wire the Staff / Client login

Login buttons go to `CONFIG.LOGIN_URL` (search `index.html`):

```js
const CONFIG = {
  LOGIN_URL: 'http://localhost:3000/login',          // dev (current)
  // LOGIN_URL: 'https://portal.mpactcap.com/login', // suggested prod
  // LOGIN_URL: 'https://mpact-fund-management.vercel.app/login',
};
```

Both Staff and Client buttons hit the same `/login` URL — the
fund-management app routes by `user_type` after AAL2
(STAFF → `/dashboard`, CLIENT → `/portal`). The button click appends
`?role=staff|client` so login copy can vary if desired.

**Update when:** the `mpact-fund-management` Next.js app is deployed
to Vercel. Suggested path: deploy to Vercel, point
`portal.mpactcap.com` (Namecheap CNAME) at the Vercel deployment,
then change `CONFIG.LOGIN_URL` to `https://portal.mpactcap.com/login`,
commit + push. Pages rebuilds in ~1–2 min.

## Edit workflow

```bash
cd C:/Users/token/mpact-capital-website
# edit index.html / etc.
git add -A
git commit -m "your message"
git push origin main
# Pages rebuilds in ~1–2 min
```

## Brand system

| Token            | Hex         | Used for                                |
| ---------------- | ----------- | --------------------------------------- |
| `--ink-900`      | `#0A0820`   | Footer / deepest darks                  |
| `--ink-800`      | `#150F33`   | Logo background — primary dark          |
| `--ink-700`      | `#221A4F`   | Card backgrounds on dark                |
| `--magenta`      | `#FF00C8`   | M wordmark / primary CTA                |
| `--magenta-700`  | `#C30099`   | Magenta hover                           |
| `--orange`       | `#FF7B1C`   | "Pact" wordmark / secondary CTA         |
| `--cyan`         | `#00C7FD`   | "Capital" wordmark / info & links       |
| `--yellow`       | `#FFD900`   | Sparing accent (dot, gradient)          |
| `--red-pink`     | `#FF0A64`   | Sparing accent                          |
| `--cream-50`     | `#FFF8F2`   | Light section background                |

Typography: **Fraunces** (display serif) + **Inter** (UI/body),
both via Google Fonts.

## Regulatory

- CRD #335008
- SEC #801-135352
- Form ADV (firm-specific IAPD page) is linked in the footer:
  https://adviserinfo.sec.gov/firm/summary/335008

## Source of content

- Divisions, team bios, advisory bios: from mpactcap.com sub-pages
- News items: ImpactAlpha, EIN Presswire, ESG Dive, Economic Architecture
  Project, PR Newswire, Yahoo Finance — links in the News section
- Logo + transparents: `G:\Shared drives\MPC Logo & Media Files\
  Logo & Media Files\`
- Partner logos: Avalanche (cryptologos.cc), Hedera (cryptologos.cc),
  Homium (typeset locally — drop a real SVG into
  `assets/partner-homium.svg` to override)

## Outstanding before launch

- [ ] DNS pointed at GitHub Pages (manual, see above)
- [ ] Enable HTTPS-only after cert provisions
- [ ] Update `CONFIG.LOGIN_URL` to the deployed dashboard URL
- [ ] Replace draft Privacy Policy and Terms with attorney-reviewed
      versions
- [ ] (Optional) Add real article links for `#` news entries if any
      remain
