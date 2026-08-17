# foundlingllc.com

Static marketing site for Foundling LLC, a New York limited liability company providing employer of record, staffing and placement, and payroll administration services from Granville, New York.

Plain HTML and CSS. No framework, no build step, no JavaScript. Every page works when opened directly as a file and with CSS disabled.

## Local preview

No tooling is required. Either:

- Open `index.html` directly in a browser, or
- Serve the folder with any static server, for example:

```bash
npx --yes http-server . -p 8080
```

Then open http://localhost:8080.

## Deploying to GitHub Pages

1. Create a GitHub repository (for example `foundlingllc.com`) and push this folder to the `main` branch:

```bash
git init && git add -A && git commit -m "Initial site" && git branch -M main && git remote add origin <your-repo-url> && git push -u origin main
```

2. In the repository, go to Settings, then Pages. Under "Build and deployment" choose "Deploy from a branch", branch `main`, folder `/ (root)`.
3. Under "Custom domain" enter `foundlingllc.com` and save. The `CNAME` file in this repo keeps that setting across deploys.
4. After DNS is in place (below), check "Enforce HTTPS". Certificate issuance can take up to a day after DNS propagates.

## DNS records at GoDaddy

For the apex domain plus www on GitHub Pages, create these records in GoDaddy's DNS management for foundlingllc.com:

| Type  | Name | Value                     |
|-------|------|---------------------------|
| A     | @    | 185.199.108.153           |
| A     | @    | 185.199.109.153           |
| A     | @    | 185.199.110.153           |
| A     | @    | 185.199.111.153           |
| AAAA  | @    | 2606:50c0:8000::153       |
| AAAA  | @    | 2606:50c0:8001::153       |
| AAAA  | @    | 2606:50c0:8002::153       |
| AAAA  | @    | 2606:50c0:8003::153       |
| CNAME | www  | `<your-github-username>.github.io` |

Notes:

- Delete GoDaddy's default parked A record on `@` and any default `www` forwarding first; they conflict.
- The AAAA records are optional but recommended (IPv6).
- In the GitHub Pages settings, also add `www.foundlingllc.com` handling by leaving the custom domain as `foundlingllc.com`; GitHub redirects www to the apex automatically once the CNAME record exists.

## Design token system

All tokens live in `:root` at the top of `css/site.css`.

**Palette.** A cool institutional scheme, chosen to read like a firm that predates the web and to avoid both the current AI-default look (warm cream, high contrast serif, terracotta) and anything that could be read as a cannabis brand:

- `--ink #1c272c`: a deep cool neutral used for body text, header, and footer. Near-black without being black, which keeps long text comfortable.
- `--paper #f4f6f7` and `--paper-2 #e9edef`: cool paper surfaces. Deliberately gray-blue rather than cream.
- `--signal #17546e`: a deep petrol blue used only for links and rules, per the brief. It never fills a surface. Measured contrast on paper is about 7.6:1, above WCAG AA for normal text.
- No greens anywhere. No gradients, no shadows, no rounded corners beyond the browser default of zero.

**Type.** Georgia (a system serif, sober and slightly characterful) for the wordmark and display headings; the Segoe UI and system-ui stack for body text. System fonts were chosen deliberately: no webfont request, no layout shift, works offline and with strict content blockers, and the pairing reads as an established professional services firm rather than a startup.

**Signature element.** The "ledger rule": a short double rule in the signal color under every section heading (`.ledger`). It is the only decorative device on the site, a quiet reference to ruled payroll ledgers. Everything else is hairlines, whitespace, and type.

**Spacing and layout.** A small spacing scale (`--s-1` through `--s-6`), a 68rem container, and a 44rem measure for running text. Grids use `auto-fit, minmax()` so every layout works from 320px to 1600px without breakpoint sprawl; only two small media queries exist (reduced motion and sub-400px padding).

**Accessibility.** Visible `:focus-visible` outlines on every interactive element (paper-colored on the dark header and footer, signal-colored on paper), `prefers-reduced-motion` disables all motion, one `h1` per page with correct heading order, and semantic HTML throughout. All text colors were checked against their backgrounds at 4.5:1 or better.

**No JavaScript.** The brief allows `js/site.js` only if actually needed. The navigation is a plain wrapping link list rather than a hamburger menu, so nothing on the site needs JavaScript and the file was intentionally not created.

## Placeholders

None. No `{{TODO: ...}}` placeholders were needed; every fact on the site comes from the company facts provided. If a street address or phone number is added later, contact.html and the JSON-LD block in index.html are the places to update.

## Open Graph image

`assets/og-image.svg` is the source for the social sharing image. Social crawlers do not render SVG, so convert it to PNG at 1200x630 before deploy and save it as `assets/og-image.png` (all pages already reference the PNG path). Any converter works, for example:

```bash
npx --yes sharp-cli -i assets/og-image.svg -o assets/og-image.png resize 1200 630
```

## File inventory

- `index.html`, `services.html`, `industries.html`, `employers.html`, `careers.html`, `about.html`, `contact.html`, `404.html`
- `css/site.css`: the single stylesheet with the token system
- `assets/logo.svg`, `assets/favicon.svg`, `assets/og-image.svg`
- `CNAME`, `robots.txt`, `sitemap.xml`
- JSON-LD `Organization` markup on the home page only, with name, url, email, and address locality. No `Person`, `founder`, or `employee` nodes anywhere, by design.
