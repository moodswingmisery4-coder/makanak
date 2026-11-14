# Makanak — session state

Last updated: 2026-11-14 (local machine time zone)

## Live surfaces
- Frontend (GitHub Pages): https://moodswingmisery4-coder.github.io/makanak/
- Frontend repo: https://github.com/moodswingmisery4-coder/makanak (branch main)
- Local frontend: D:\hermes\makanak
- Backend repo/local: D:\hermes\makanak-worker (Cloudflare Worker, module syntax)
- API base 1 (primary): https://makanak-worker.makanak-app.workers.dev/api
- API base 2 (fallback): https://makanak.kdns.fr/api  ← blocked on Soheil's mobile LTE; works elsewhere
- D1 database: makanak-db (6d0fd00a-a76b-401b-9baf-9f31c55f2b45), tables users / sessions / bookings

## Verified working
- Registration, login, token session (localStorage `makanak_token`), logout
- Booking create from stay.html and listing in the user panel
- Anonymous "درخواست رزرو" → prompts login
- 84 stays, 453 same-origin gallery photos (assets/gallery), 40 popular stays over 4 pages
- Card click navigates straight to stay.html?id=... (no intermediate modal on index)
- City dataset 1628 cities with Persian district labels ("تهران، منطقه ۱")

## Latest UI state (deployed, commit 14328c5)
- Destination field is 2fr on a 1.7fr/1.3fr/1.3fr/1.05fr/auto desktop grid; compact paddings
- Date fields ~203px, guest ~164px, search button min 118px, all single-line with the × visible
- City dropdown panel: 330px wide; chips 75×23, font 11.52px, 4 per row, 0 truncated
- Calendar panel height now fits content (221px desktop, 266px mobile), no dead space

## Open / unconfirmed
- Soheil was NOT satisfied with the last chip/panel sizing pass ("ریدی"); the numbers are live
  but he never confirmed the look. Re-check visually before further changes; ask him to point
  at what is still wrong instead of guessing.
- Test accounts created during E2E still exist in D1 (emails starting e2e/dbg/verify/flow/tok/final).
  Cleanup needs his explicit consent (the delete command was blocked once).
- Deployment quirk: `wrangler deploy --domains ...` disables the workers.dev route. Use plain
  `npx wrangler deploy` (config has workers_dev = true) to keep both endpoints alive.
- kdns.fr custom domain requires Cloudflare SSL to be fully issued; it works from Ukraine/EU nodes
  but not from his carrier.

## Commands
- Deploy backend: `cd D:/hermes/makanak-worker && npx wrangler deploy`
- Apply schema: `npx wrangler d1 execute makanak-db --remote --file=schema.sql`
- Push frontend: `cd D:/hermes/makanak && git add -A && git commit -m "..." && git push origin main`
  (if TLS handshake fails: `git -c http.version=HTTP/1.1 push origin main`)
- Bump `assets/auth.js?v=N` and `assets/catalog.js?v=N` whenever those files change.
