# Retail Acquisition Explorer

Map up front with every property pinned, sortable table below it, full
breakdown panel on click.

## Deploy it

1. Push this folder to a new GitHub repo.
2. Import it in Vercel ([vercel.com/new](https://vercel.com/new)) — Framework Preset: **Other**. Deploy.

That's it — no build step, no environment variables, no serverless functions.
Every file here is served as-is.

## The password gate

Code: **DAN**. This is a client-side check baked into `index.html` — it hides
the page behind a password prompt, remembers you for the browser tab's
session (`sessionStorage`), and that's it.

**Be clear about what this is and isn't:** it's enough to keep casual
visitors and search engines out (there's also a `noindex` tag), but it is
**not real security** — the entire page and its data are downloaded to the
browser either way, so anyone who opens dev tools and views source can see
everything regardless of the password. Don't put anything here you wouldn't
be comfortable existing as a public, unlisted link.

To change the password, open `index.html`, search for `SITE_PASSWORD`, and
edit the string. Commit and push — no rebuild, no redeploy step beyond that.

Want real, server-side password protection instead (so the data itself is
gated, not just the view)? That's a bigger step up — it means introducing a
backend of some kind (Vercel Edge Middleware + a serverless function is the
usual pattern, or Vercel's own paid Password Protection feature if you're on
a Pro/Enterprise plan) — happy to build that out if/when you want it, just
say so.

## Adding Offering Memorandums

No live upload from the browser (that needs a file-storage backend, which
plain static hosting doesn't have). For now:

1. Drop the PDF in `/oms`.
2. Add one line to `oms.json` mapping that property's `id` (from `data.json`) to the file's path.
3. Commit, push — Vercel redeploys automatically and the property's detail panel picks it up.

## Files

```
index.html   Map, filters, table, detail panel, and the password gate
data.json    The 365-property dataset
oms.json     Property id → OM PDF path
oms/         OM PDFs live here
```
