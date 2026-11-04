# Smaart Care — online coloring portal UI (smaartcare.online)

Single-page site. Deploy this folder to Vercel as-is (vercel.json rewrites all routes to index.html).

## Shareable plan URLs
- /pricing            all plans
- /pricing/1-month    $35
- /pricing/3-months   $100
- /pricing/6-months   $180 (+1 month free)
- /pricing/12-months  $350 (+2 months free)
Other pages: /login, /portal, /admin, /contact, /terms-of-service, /privacy-policy, /refund-policy (also /termsofservice, /privacypolicy, /refundpolicy)

## What already works in the UI
- Coloring studio: tap-to-fill, brush (3 sizes), eraser, undo/redo (Ctrl+Z / Ctrl+Shift+Z), start over, 24 colors
- Example (reference) picture beside the canvas
- Submit: score out of 10 = share of parts whose color family matches the example (computeScore/famOf), celebration popup with stars + confetti
- Save with a name, rename, continue later, delete (My artwork)
- Admin: upload an SVG drawing -> parsed into tap-to-fill parts -> appears in the portal
- Marketing copy promises 50+ new drawings every month
- 150 built-in drawings (50 subjects x Easy/Medium/Hard) across Animals, Ocean, Sky, Nature, Vehicles, Toys, Food;
  spread 50 per month over Sep/Oct/Nov 2026. Same data exported in drawings.json to seed the database.
- Portal filters: category, level, search, 'Show more' paging (24 at a time)
- Admin: bulk upload (select many SVGs at once; names come from file names)

## Integration points (search "developer" in index.html)
1. PLANS: fill paddlePriceId / polarProductId; startCheckout() opens Paddle or Polar checkout (all auto-renew).
2. Payment webhook: create account, generate password, email login ID + password.
3. Auth: real login + session check (replace `loggedIn`); protect /portal and /admin.
4. DRAWINGS: seed the database from drawings.json, then load from the database (replace `buildDrawings()`). Format: {id,name,cat,level,month,vb,parts:[[tag,attrs,refColor|'line']]}.
   Admin upload uses parseSvg() to build `parts` — store that JSON.
5. ART (saved artwork): preview stores it in the browser (localStorage). Replace loadArt()/persistArt()
   with API calls so artwork is saved per account: {id,drawingId,name,score,fills,strokes,created,updated}.
6. "My subscription" buttons -> Paddle/Polar customer portal (update card, cancel).
7. Contact form -> support@smaartcare.online.
8. Remove the "Preview data" label once real data is connected.

## SVG rules for new drawings
Each part must be a closed shape (path/circle/rect/ellipse/polygon). Shapes with fill="none" become outline-only lines.
Avoid nested <g transform> groups — flatten transforms before exporting (Inkscape: Path > Object to Path, ungroup).
