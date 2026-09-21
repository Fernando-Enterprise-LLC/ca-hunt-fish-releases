# `live/` — the version-independent lane

Nine files, republished on their own cadence by `pipeline publish-live`, at a path that does
not carry a dataset version:

| File | What it is | Read cadence |
|---|---|---|
| `stocking_current.json` | CDFW's +/-3-week Fish Planting Schedule window, read from the page CDFW tells the public to use | daily |
| `operator_schedules.json` | Lake operators' own published schedules, quoted with attribution | weekly |
| `waterfowl_results_current.json` | CDFW's waterfowl hunt results for the season CDFW's own page says it is publishing — or `pending` when that heading carries no document yet | weekly |
| `trip_pack.json` | CDFW's deer harvest statistics for the year CDFW's own page links a report for, with the department's own "as of" date on every number — and the big-game draw statistics, `pending`, because nobody has opened that document | monthly |
| `operators.json` | The attributed commercial operator directory — what each operator publishes about itself, quoted with the date it was read | monthly, one page per host |
| `notices_current.json` | In-season change announcements: the agency's own headline, date, category and at most two of its own sentences, with the records a declared map ties them to | on a change, with a daily floor |
| `beach_status_current.json` | What a county says about a beach today, quoted, with the county named and the time this project read it — plus, for every county in the beach directory, the hotline and status page the county itself tells the public to use | every three hours |
| `verification_current.json` | THE RECEIPT: every source the daily lane re-read, the moment it read it, the digest of the bytes it compared against, and one of three words — `unchanged`, `changed_pending_review`, `unreachable` | daily |
| `manifest.json` | The sha256, byte count, read date and staleness threshold of each of the above | every run |

These are NOT a dataset release. Releases are immutable under `v/<version>/` and reach the
app through `manifest.json` at the repo root. These are a small, version-independent sidecar
the app may refresh while a screen that reads one of them is open.

Every row in these files carries the source it came from and the date this project read it.
CDFW rows say CDFW; operator rows say "<operator> states". Nothing here is inferred, and an
empty week is a stated absence with a reason, not a blank.

`waterfowl_results_current.json` is columnar and carries no sentence: the app builds every
line from the typed columns. Its numbers count birds and hunters, they are marked
`scoreable: false`, and they never move a match count, a band or an ordering anywhere in the
app. A season whose document CDFW has not published yet ships `status: "pending"` with no
rows — never an empty table, which reads as "nobody hunted here".

`operators.json` is the one file here that is not an agency's. Every sentence in it is what
a private business publishes about itself, quoted with the URL and the date this project read
it, and **this app verifies no licence** — California publishes a lookup for guides and no
public list of licensed charter vessels at all, and the file says so in its own bytes. There
is no rating, review, ranking or score field in its schema; the ordering is distance from the
place on screen and then name, and that sort key is printed for the reader. No booking link
carries a query string, so none can carry an affiliate tag. It also carries, in its own bytes,
the sentence that says what it is not: this is the operators this app was able to read from
their own published pages, not a list of the operators near you.

`notices_current.json` carries what an agency SAID and never what a rule now is. **No
regulation value in this app moves on that lane.** Every string in it is one of four
things: a verbatim field an agency wrote, a template the app resolves, a curated basis a
person wrote and signed, or a structural token. There is no summary field and no field a
paraphrase could be written into. A record is tagged as possibly affected when a phrase
declared in a curated map appears in the agency's own title or quoted sentences — exact,
case-folded, unstemmed — and the written reason for the tag travels beside it and is shown.
Nothing leaves the file without a stated reason: a notice the source has dropped on two
consecutive reads, or one that has reached this app's own 180-day cap, is marked expired
with its reason and stays for thirty days. A read that fails publishes nothing at all and
leaves the notices already here wearing their own read date, because an empty list would
tell every reader that nothing has been announced.

`verification_current.json` says what this project RE-READ and never what it found in the
text. Every row is one source: the address, the moment the request was made, the sha256 and
fetch time of the shipped snapshot it was compared against, and one of three words. **A
source that changed shows no new value anywhere in this app** — the redline and promote path
is the only thing that moves a value and it ends at a person — so `changed_pending_review`
means a change is under review and carries the day this project saw it, and the app goes on
showing the text it last checked. A source that could not be reached says `unreachable`, which
is a different fact from `unchanged` and used to be indistinguishable from it. The file's own
two-day clock is the only way freshness can lapse now, and past it the app says its daily
check has not run and names the date. The domains this lane does NOT re-read are listed in the
file's own `coverage.domains_not_checked`, each with the reason, because "never looked at"
must never read as "checked and unchanged".

`beach_status_current.json` carries a COUNTY's own words and never this app's opinion of
them. Every status ships as the county published it, byte for byte, in quotation marks, with
the county named and the time this project read it beside it; the one normalisation is a
`kind`, which travels beside the verbatim it came from and is never rendered as a word. A
county this app does not read is `absent` with a stated reason — never `open`, and never
silence — and a county whose service did not answer keeps the status this app last read,
wearing the time it was read. A county all-clear may never clean a measured exceedance in
`beach_water_quality`: the two are separate instruments and the county's word only ever
raises. Fifteen of the sixteen counties in this file are link-outs; the file says so in its
own bytes, county by county.

`trip_pack.json` is columnar on the same terms, and its numbers count deer and tags. Where
CDFW prints a figure as the total for a GROUP of zones — B-zone, C-zone and D3-5-zone tags
are issued for a group — the row says so and the app prints it as the group's figure, never
as that one zone's. Nothing in the file is computed: no rate, no trend, no comparison. Its
draw-statistics block is `pending` with no rows AND no column names, because nobody has
opened that document and a placeholder naming a column that may not exist is how one gets
implemented from memory later.
