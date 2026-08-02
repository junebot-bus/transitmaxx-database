# transitmaxx-database

Combined transit database for **TransitMaxx** (`com.junebot.transitmaxxbusmap`) —
every city the Next Bus family covers, in one Core Data store.

| | |
|---|---|
| Stops | 212,900 |
| Routes | 4,796 |
| Agencies | 68 |
| Cities merged | 24 |

## Files

- `dcbusmap.sqlite.gz` — gzipped Core Data store (59 MB compressed, 266 MB expanded)
- `dcbusmap_version.txt` — version string the app compares against its bundled value

**The store is gzipped, unlike the per-city repos**, which serve their `.sqlite`
uncompressed. At 266 MB the raw file exceeds GitHub's 100 MB limit; gzip brings it
to 59 MB, which keeps the same `raw.githubusercontent` delivery path the other
cities use. Clients must gunzip after downloading.

## Building it

    python3 scripts/build_combined_database.py

Merges every `*/Resources/*busmap.sqlite` in the app repo. Rows are keyed by
`(agency, stopID)` and `(agency, routeID)`, which deduplicates the two agencies
deliberately built into two cities:

- `marcRail` — 79 identical stops in both DC and Baltimore
- `mtaMdCommuterBus` — 193 in Baltimore + 299 in DC, unioning to the full
  477-stop, 36-route network that the per-app terminus split had divided

That split exists only because neither city app could sensibly show the whole
network; the combined build removes the limitation by construction.
