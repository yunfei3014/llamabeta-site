# Beta Fund — betafund.ai

Marketing site for **Beta Fund**, the fund for the agent-native era.
$200K first check, up to $2M across pre-seed and seed. Global.

## Architecture

Plain **static multi-page** site — no build step, no framework, no runtime.
Every page is a standalone `.html` file sharing one stylesheet and one script.

```
index.html        Home — hero, conviction film, explore cards, CTA
thesis.html       Thesis — token-native / agent-native / self-improving
companies.html    Portfolio — 4 themes, 12 companies
fellowship.html   The Fellowship — "more than capital" + what's included
apply.html        Application form (posts to the Butterbase apply fn)
css/site.css      Shared design system (mobile-first)
js/site.js        Shared behavior — mobile nav, reveals, canvas motifs, form
assets/           Images
```

This replaces the previous single-page Butterbase DC-runtime build. It now
works on any static host (GitHub Pages, Butterbase static, S3, Netlify…) with
no client-side runtime dependency, and is fully responsive with a real mobile menu.

## Local preview

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

## Application form

`apply.html` POSTs JSON to a Butterbase serverless function, which persists the
application server-side. The endpoint and the field id → payload key mapping live
in `js/site.js` (`MAP`). No credentials are needed or stored client-side.

## Deploy (Butterbase static)

Zip the site contents at root and deploy to app `app_oh23tcj73owo`, framework
`static`:

```bash
cd betafund-site-mpa
zip -r frontend.zip index.html thesis.html companies.html fellowship.html apply.html css js assets
# create_frontend_deployment {app_id, framework:"static"} -> uploadUrl
# curl -X PUT "<uploadUrl>" -H "Content-Type: application/zip" --data-binary @frontend.zip
# manage_frontend {action:"start_deployment", deployment_id}
```

## Design tokens

Cream + vermilion editorial. Edit in `css/site.css` `:root`:
`--accent:#E04E26` · `--accent-hover:#C43D18` · `--accent-deep:#8F2E10` · `--ink:#000` · `--paper:#F5F2EC`.
Fellowship (`fellowship.html`, `body.theme-green`) overrides to a light cream + green look:
`--accent:#1B9E4B` · `--accent-hover:#008543`, ink text `#17181A` on a `#F6F4EE` background.
Type: Helvetica Neue (display/body) + Space Mono (labels/mono).
