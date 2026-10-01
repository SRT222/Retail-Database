# Retail Acquisition Explorer

A map-first tool for browsing retail acquisition listings and sale comps across
Southern California and the Inland Desert West (Las Vegas metro, Eastern
Sierra). Map up top with every property pinned, sortable table below it, full
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

## Listings vs. Sales Comps

The toolbar at the top has two modes, working like a toggle:

- **Listings** — active properties currently for sale (this is the default on load).
- **Sales Comps** — closed/recorded sales, with price, $/PSF, cap rate, sale
  date, buyer/seller, and the original transaction notes.

Clicking the already-active button turns it off and shows **both together** —
listings and sale comps on the same map and in the same table, color-coded
and tagged (For Sale / Sold) so you can tell them apart. All filters (search,
Building SF, Price, Land Size, $/PSF, county) apply to whichever mode is
active; Sales Comps and the combined view also get a Sale Date range filter
that Listings doesn't need.

Every listing's detail panel also shows **sale comps within 1 mile** — the 5
most expensive by default, with a "see more" expand if there are more. Click
one to jump straight to that comp's full detail view; the **×** button takes
you back to the listing you came from instead of closing everything.

## Southern California vs. Inland Desert West

Two more toggle buttons sit right under the sourcing-criteria line, working
the same way as Listings/Sales — click one to zoom/filter to that region,
click it again to go back to showing everything. Selecting a region also
narrows the county chips to just the counties in that region.

- **Southern California**: LA, Orange, San Diego, Riverside, San Bernardino,
  Ventura, Santa Barbara, San Luis Obispo, Kern, Imperial.
- **Inland Desert West**: Clark County NV (Las Vegas metro), Washoe County NV
  (Reno/Sparks), Mono and Inyo Counties CA (Eastern Sierra).

## County Quick Stats

Select one or more county chips and a "📊 County Quick Stats" button becomes
clickable (it also lives on every property's detail panel, pre-filtered to
that property's county). It opens a full demographic/market breakdown per
county — population, age, race/ethnicity, education, household income,
housing, disposable income brackets, net worth brackets, and consumer
expenditure by category — sourced from Esri via CBRE. If multiple counties
are selected, it shows a tab per county.

## Favorites, Notes, and Saved Views

- **Favorites (♥)** — toggle from the table or detail panel. Works across
  both Listings and Sales Comps; "Export favorites to Excel" pulls from both
  into one sheet, tagged by record type.
- **User Notes** — per-property freeform notes, editable from the detail panel.
- **Saved Views** — "+ Save current view" captures every active filter
  (including which mode, which region, county/tenancy selections, sort, and
  date range) under a name you choose. Capped at 10; "View saved views"
  lists them with one-click load and delete.

All three of these save to **`localStorage`** — per-browser/per-device only,
not shared across users or devices. Shared/synced favorites, notes, and views
would need a real database backend (not built).

## Adding Offering Memorandums

No live upload from the browser (that needs a file-storage backend, which
plain static hosting doesn't have). To attach one:

1. Drop the PDF straight in the repo root (e.g. `57980-twentynine-palms-hwy.pdf`, right next to `index.html`).
2. Add one line to `oms.json` mapping that property's `id` (from `data.json`) to that filename — no path prefix, just the filename.
3. Commit, push — Vercel redeploys automatically and the property's detail panel picks it up.

## Adding flyer images

Flyer/excerpt images live in the **`images/`** folder (not the repo root —
they used to be flat in root, but were moved into this folder to keep the
root listing manageable as the image count grew into the hundreds). To add
more:

1. Drop the `.jpg` file(s) into `images/`, named `excerpt-<property id>-<N>.jpg`
   (e.g. `excerpt-p710-1.jpg`, `excerpt-p710-2.jpg` for a second page).
2. Add the filename(s) to that property's `excerptImages` array in `data.json`
   (listings) or `sales-comps.json` (sale comps).
3. Commit, push.

## Files

```
index.html             Map, filters, table, detail panel, password gate, everything
data.json               Listings dataset (Southern California + Inland Desert West)
sales-comps.json        Sale comps dataset, same two regions
demographics.json       County-level demographic/income/net-worth/expenditure data
oms.json                Property id → OM PDF filename
images/                 Flyer excerpt images (excerpt-pNNN-N.jpg), flat inside this folder
README.md, .gitignore
```

## Known limitations

- Password gate is cosmetic, not real security (see above).
- Favorites, Notes, and Saved Views are per-browser (`localStorage`), not
  synced across devices or users.
- The sourcing-criteria text in the ribbon ("as of 9/28/2026" / "sold after
  1/1/2026") is static — it won't update itself as time passes or as new data
  is added outside those windows.
- "Sale Comps Within 1 Mile" uses a fixed 1-mile radius — this can return
  zero comps in low-density areas (Eastern Sierra, rural desert) and quite a
  few in dense urban areas (LA, Las Vegas). This is an intentional simple
  tradeoff, not a bug.
- Sale comps don't have flyer images or Offering Memorandums attached —
  those features are listings-only.
- Some sale comps are missing Tenancy, Cap Rate, or Transaction Notes —
  CoStar's own export didn't have that data for every record; the detail
  panel shows "Not reported" rather than guessing.
