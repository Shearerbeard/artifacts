# Kid PWA mockups (S8)

Gate U input for S8 (Kid PWA). Six static HTML mockups, one per app
state, each openable on its own in a browser. The phone frame is drawn
at 390px wide, so what you see on desktop is the phone width. Nothing
is interactive; buttons and links are inert. Light and dark both work:
each file follows your OS `prefers-color-scheme`.

## How to review

Open each file below and check it against two questions. First, could
a kid who reads at a grade-1 level do the right thing without help?
Second, does the screen show exactly one thing, with no way to wander
off? The product rules under review come straight from the card: a kid
must never see a sibling's data, and there is no list of chores to
browse. Swapping is not part of this app.

| State | File |
|---|---|
| 1. Card view | [mockup-01-card-view.html](mockup-01-card-view.html) |
| 2. Done flow | [mockup-02-done-flow.html](mockup-02-done-flow.html) |
| 3. Photo upload | [mockup-03-photo-upload.html](mockup-03-photo-upload.html) |
| 4. Rejected with notes | [mockup-04-rejected.html](mockup-04-rejected.html) |
| 5. Empty state | [mockup-05-empty.html](mockup-05-empty.html) |
| 6. Returned to pool / all caught up | [mockup-06-returned.html](mockup-06-returned.html) |

## The states

### 1. Card view

The kid sees their one assigned chore. A large title sits at the top,
then the location in the house. Steps appear as numbered rows with
circled numerals, and a strip of check-off circles sits under them as
the checklist graphic. An optional "Watch how" link appears when the
card carries a how-to video. A due line ("Finish by Saturday") shows
only when the card has a due date. The single action is the stamp: a
large circular Done button at the bottom. State 1b (same file) shows
the after-submit variant. The stamp renders disabled between "All
sent!" above it and "{ParentName} will check your photos." below; the
parent-name fallback is the one the reject/approve copy already uses.
Added 2026-08-18, approved at the U(code-review) gate.

### 2. Done flow

Tapping Done opens a bottom sheet over the dimmed card view. "Take a
photo to show the work." states the requirement up front, because the
domain requires photo evidence (`Submission`, photos mandatory, max
10). Add photos continues to the upload screen; Cancel returns to the
card. One interruption, not a wizard.

### 3. Photo upload

A two-column grid of thumbnails, each with a remove control ("✕",
aria-labeled "Remove photo", 44px target). The helper line states the
1-10 range, and a counter shows "{count}/10 photos". An "Add more"
tile sits in the grid. Finish submits; Back returns to the card view.
Validation copy ("Add at least one photo." / "Too many photos. The
limit is 10.") appears inline above Finish.

### 4. Rejected with notes

When a parent rejects the work, the kid sees "Not done yet" with the
parent's name: "Dad says it needs more work." The parent's notes
appear in a bordered block with a red edge. The only action is "Try
again", which returns to the card view so the kid can redo the work
and resubmit. There is deliberately no "ask for help" affordance: no
notification path exists yet (that is S15 scope), and the card names
only notes plus retry here.

### 5. Empty state

The landing state when the kid holds no assignment: "Nothing to do
yet" and one button, "Get my chore", which triggers the draw. The QR
code is the URL itself, so there is no scan step inside the app. The
draw lands on the card view, or on "All caught up" when the pool is
empty.

### 6. Returned to pool / all caught up

Two moments share this file. After approval, a green stamped ring sits
over the card's stamp and the kid sees "Nice work! Dad says it is
done." with "Get my chore" to draw again. When a parent returns a
drawn chore to the pool, the body reads "That chore went back to the
pile." with the same draw button and no stamp ring. Separately, a draw
that finds no Ready cards (the typed `NoChoresAvailable` outcome)
shows "All caught up. No chores right now. Check back later." with a
"Check again" button that retries the draw.

## Design decisions on record

- **Server-rendered, axum + templates.** The mockups demand no client
  framework: every state is one server-rendered page, and the only
  interactions are form posts (draw, upload, finish). The choice and
  this reasoning land in the card log per deliverable 6 when
  implementation starts.
- **Design tokens live in the repo.** `design/philosophy.md`,
  `design/tokens.json`, and `design/tokens.css` are the single source
  for color, type, and space. The mockups inline the generated values
  so each file opens standalone; the real templates will consume
  `tokens.css` directly. The signature element is the rubber-stamp
  Done button; step numerals sit in ink circles.
- **Copy is keyed.** Every user-facing string comes from
  [copy-deck.md](copy-deck.md) under a stable key, so implementation
  templates reference keys rather than inventing wording. The deck
  records the board-owner amendments of 2026-08-14 (no help button,
  draw-driven empty state, busy string, returned-state strings).
- **Token-to-kid resolution: fold on read.** The API folds the kid
  event category and matches `QrToken` on each request; no new store
  in this card. A persisted kid-view projection waits until the UI
  shape is locked (user decision, 2026-08-14).
- **Photos land on the filesystem.** Uploads go to a configured
  directory (`CHORE_KID_UPLOAD_DIR`), with `AssetRef` pointing at the
  path; parent review reads the same paths. No blob store in this
  card (user decision, 2026-08-14).
- **No offline cache.** The service worker exists only to make the
  PWA installable; home-wifi-only is the recorded product decision.
- **LAN-only.** The listener binds an address that is not exported
  beyond the LAN, enforced in server config and asserted by a test or
  machinery check during implementation.

## Out of scope (deliberate)

The token URL is the only credential. A wrong or absent token gets a
404, and there are no logins or accounts. Payloads never carry sibling
data. Chore history stops at the current card. Gamification stays out
too; the only flourish is the quiet stamp ring on approval.

## Gate A review ledger (2026-08-14)

- Authors: kimi-for-coding/k3 (board owner: HTML shells, design
  tokens, this index) and amazon-bedrock/deepseek.v3.2 (prose-write:
  the copy deck).
- Reviewer: openai/gpt-5.5 through the `general` agent, fresh context,
  session ses_ffdcdefa1ffewcvdbYOPWpiIjX. Different family from both
  authors, satisfying the reviewer-differs-from-author invariant.
- Verdict: PASS.

Findings:

1. MINOR - this index implied swapping might exist after parental
   approval ("Swapping stays impossible until..."). Disposition: fixed
   in f104d2d; the rule now reads "Swapping is not part of this app."

Checks the reviewer could not run are unverified, not findings:

- On-device behavior (rendered contrast, touch targets) belongs to
  Gate M and the wave-close Gate T.
- The voice gets its real test from the user at Gate U; no formal
  reading-level score was attempted.
- Token parity between tokens.json and the inlined mockups was
  spot-inspected at Gate S. The real templates consume tokens.css
  directly, so the duplication disappears with the mockups.
