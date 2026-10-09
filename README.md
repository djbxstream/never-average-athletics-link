# Never Average Athletics — Link Resolver

A static GitHub Pages site that resolves the current Cloudflare Quick Tunnel URL for the
Never Average Athletics website and redirects visitors to the live application.

- **Public URL:** `https://<owner>.github.io/never-average-athletics-link/`
- **Serves:** `index.html` (redirect page), `public-url.json` (current tunnel URL)
- **Hosting:** GitHub Pages, from the `main` branch

## How it works

1. The Never Average Athletics website runs a Cloudflare Quick Tunnel for public previews
2. When the tunnel starts, its URL is written to `public-url.json` in this repository
3. This repository is published via GitHub Pages
4. Visitors opening the stable GitHub Pages URL are automatically redirected to the current live website
5. When the tunnel restarts with a new URL, the publishing script updates `public-url.json` and pushes to GitHub

## Files

| File | Purpose |
|------|---------|
| `index.html` | Visitor-facing redirect page with auto-redirect and fallback button |
| `public-url.json` | Current tunnel URL. **Written by the publishing script, not by hand.** |

## Updating the tunnel URL

The tunnel URL is **not** edited manually. It is written by the publishing script
that runs as part of the website's preview startup:

```bash
# From the website project
npm run preview:start
```

That script (in the website project) reads the Cloudflare tunnel URL, validates it,
rewrites `public-url.json` here, commits, and pushes. The redirect page does not
interfere with that flow.

## Security & Access

This is a public redirector. It does not implement any access control:
- The page source is public
- The tunnel URL in `public-url.json` is public
- The Cloudflare Quick Tunnel is reachable directly

For production use, consider:
1. **Cloudflare Access / Zero Trust** — put an identity provider in front of a named tunnel hostname
2. **A stable named Cloudflare Tunnel** on a custom hostname instead of a Quick Tunnel
3. **Reverse proxy authentication** if behind an existing web platform

## License

Never Average Athletics internal utility. Contains no credentials or private data.