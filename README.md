# Example Websites for GeoScored

Fictional brand websites used as scan targets for GeoScored example reports and Easter egg campaigns (DR-123).

## Sites

| Folder | Domain | Pages Project | Purpose |
|--------|--------|---------------|---------|
| `ridgeline-climb/` | ridgelineclimb.com | ridgeline-climb | Screening example (score ~56) |
| `halcyon-agency/` | halcyon-agency.com | halcyon-agency | Full audit example (score ~50) |
| `foxglove-bakery/` | foxglovebakery.com | foxglove-bakery | WAF-blocked example |
| `signal-and-noise-geo/` | signalandnoisegeo.com | signal-and-noise-geo | Demo org (score ~90+) |

## Stack

- Plain HTML + Tailwind CSS (CDN, no build step)
- Hosted on Cloudflare Pages
- GitHub Actions deploys each folder to its Cloudflare Pages project on push to main

## Deploying

Each subfolder is an independent Cloudflare Pages project. On push to `main`, the GitHub Actions workflow deploys only changed folders.

Manual deploy:
```bash
source ~/.cloudflare/geo-pages-token
export CLOUDFLARE_API_TOKEN CLOUDFLARE_ACCOUNT_ID
npx wrangler pages deploy ridgeline-climb/ --project-name ridgeline-climb
```

## Easter Egg Links

Each site contains hidden links to `https://geoscored.ai/found?t=<signed-token>`. Token URLs are placeholder values until promos are seeded and tokens generated via the admin UI.

## Custom Domains

After first deploy, bind custom domains in Cloudflare dashboard:
1. Pages project → Custom domains → Add
2. Enter the domain (e.g., ridgelineclimb.com)
3. Cloudflare handles DNS if domain uses Cloudflare nameservers

## Foxglove Bakery WAF Rules

See `foxglove-bakery/WAF-RULES.md` for Cloudflare WAF rule configurations that block AI crawlers.

---

*This is a demonstration website created by GeoScored.*
