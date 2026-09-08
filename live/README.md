# `live/` — the stocking calendar's own lane

Two files, republished on their own cadence by `pipeline publish-live`, at a path that does
not carry a dataset version:

| File | What it is | Read cadence |
|---|---|---|
| `stocking_current.json` | CDFW's +/-3-week Fish Planting Schedule window, read from the page CDFW tells the public to use | daily |
| `operator_schedules.json` | Lake operators' own published schedules, quoted with attribution | weekly |
| `manifest.json` | The sha256, byte count, read date and staleness threshold of each of the above | every run |

These are NOT a dataset release. Releases are immutable under `v/<version>/` and reach the
app through `manifest.json` at the repo root. These two files are a small, version-independent
sidecar the app may refresh while a stocking calendar is on screen.

Every row in both files carries the source it came from and the date this project read it.
CDFW rows say CDFW; operator rows say "<operator> states". Nothing here is inferred, and an
empty week is a stated absence with a reason, not a blank.
