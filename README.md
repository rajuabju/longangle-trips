# Wayfarer

A self-hosted travel itinerary. Your trips, on your own Cloudflare account,
behind your own login. No vendor, no subscription, no shared database.

**→ [SETUP.md](SETUP.md) — start here. About an hour, mostly waiting for DNS.**

---

## What it does

- **Itinerary** — flights, stays, activities and transfers on a day-by-day
  timeline, with a map, drive times, and the connections between items worked
  out rather than typed.
- **Trip Check** — reads an upcoming trip and reports what does not add up: a
  night with no lodging, an impossible connection, a stay that checks out the
  day it checks in, a passport expiring before you travel.
- **Budget** — costs recorded as expenses linked to the booking they pay for,
  in any currency, converted at ECB rates. It tells you when a total covers
  only part of a trip rather than letting a partial figure read as complete.
- **Weather** — the forecast when you are close enough for one, and a five-year
  average of the same dates when you are not, clearly labelled as not a
  forecast.
- **Packing lists**, per person, reusable as templates.
- **Documents** — passports, boarding passes, insurance, behind the same login
  as the trip.
- **Card rewards** *(optional)* — progress toward annual credit-card spend
  thresholds, in dollars, nights or loyalty points.
- **A calendar feed** you can subscribe to from any calendar app.
- **Installable on a phone** as a standalone app, and works offline.

## What it runs on

| Piece                 | What                                                                                          |
| --------------------- | --------------------------------------------------------------------------------------------- |
| `public/`             | The client. Vanilla JavaScript, one file, no framework, no build step beyond content hashing. |
| `functions/api/`      | The API, on Cloudflare Pages Functions.                                                       |
| `functions/api/ext/`  | Cached proxies to free external services.                                                     |
| Cloudflare **D1**     | The database. Every table is documented in `schema.sql` .                                     |
| Cloudflare **R2**     | Trip documents. Private; served only through the API.                                         |
| Cloudflare **Access** | Who may log in. There is no password in this app.                                             |
| `tools/tests/`        | 71 suites, ~2,600 assertions. `npm test` .                                                    |

Maps, geocoding, routing, weather, holidays and currency all use free,
keyless services. **The app is fully usable without a single API key.**

## About the code

The comments are long on purpose. They record why something is the way it is —
almost always because the obvious alternative was tried and was wrong, and the
wrong version looked entirely reasonable. Read the comment before simplifying
the code.

The tests follow the same rule: many assert that a specific past mistake stays
fixed, and say which. If one fails, read its header before assuming the test is
stale.

## Making it yours

Four values are marked `CONFIGURE ME` — your home coordinates (twice), who
appears on packing lists, and any place you visit repeatedly without a booking.
[SETUP.md](SETUP.md) Part 1.8 covers them.

After that it is your codebase. There is no update channel and no telemetry;
nobody can see your install, including whoever gave you this.

## Demo data

`demo-seed.sql` loads four invented trips so you can confirm every screen works
before trusting the app with anything real. One line deletes them:

```sql
DELETE FROM trips WHERE id BETWEEN 9000001 AND 9000099;
```

## Licence

Provided as-is, for personal use, with no warranty and no support.
