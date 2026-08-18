# Parent UI mockups (S9 Gate U input)

Five states covering the S9 deliverables: watch-board management,
kid/QR management, and assignment visibility. Each HTML file opens
standalone; token values are inlined from `design/tokens.json`
(chore-chart v1). One fluid column, max-width 44rem - the same file
serves phone and laptop widths, which is the card's "minimal, works
on phone and laptop widths" requirement shown rather than asserted.

| State | File |
|---|---|
| 1. Overview | [parent-01-overview.html](parent-01-overview.html) |
| 2. Boards | [parent-02-boards.html](parent-02-boards.html) |
| 3. Board errors | [parent-03-board-errors.html](parent-03-board-errors.html) |
| 4. Kids | [parent-04-kids.html](parent-04-kids.html) |
| 5. Kid QR | [parent-05-kid-qr.html](parent-05-kid-qr.html) |

## The states

### 1. Overview

The landing page and the assignment-visibility deliverable in one.
Each kid is a bordered card: name, a state chip ("In progress", "In
review", "No card"), the current card's title, and how long they have
held it ("Held for 2 days" / "Submitted 1 hour ago"). Watched boards
sit at the bottom as a strip of pills. This is deliberately not a
board view - Vikunja stays the board; the parent comes here to see
who is holding what.

### 2. Boards

The watched-board list (name, board ID, watching-since) with a
Remove action per board, then the add form. The form takes a board ID
or a pasted Vikunja URL and names the six expected lanes up front, so
the most common failure is visible before submit, not after.

### 3. Board errors

The three validated add failures, each a bordered block with a plain
heading. Board not found echoes the ID back. Lanes missing shows the
six expected lane names as pills with the absent ones struck through
and named in prose. Already watched is a no-op explained, not an
alarm. These mirror the thiserror variants the API returns; the copy
is the error text, not a summary of it.

### 4. Kids

The kid list with two actions per kid: Show QR, and Regenerate
(marked as the caution action). The add form is a single name field
whose hint states the security model plainly. The link is the whole
login; anyone holding it sees that kid's card.

### 5. Kid QR

Full-screen QR for one kid with the token URL under it in
monospace, a Print action, and a warning block covering regenerate
(the old link dies the moment the new one is made). A print
stylesheet strips everything but the QR and the URL, so the printed
page is the thing you tape to the fridge. The QR graphic here is a
placeholder; implementation renders the real token URL.

## What is not here

- No login screen. Parent identity comes from the tailnet
  (deliverable 5's `ParentAuth` seam); there is nothing to mock up.
- No approve/reject. Work review stays in Vikunja (or the S8 CLI
  commands); this surface is management only.
- No board or lane views. Assignment visibility stops at card title,
  state, and held duration, per deliverable 4.

## Copy table

| Key | Text |
|---|---|
| `nav.sections` | Overview / Boards / Kids |
| `overview.kids_heading` | Kids |
| `overview.boards_heading` | Watched boards |
| `overview.chip_progress` | In progress |
| `overview.chip_review` | In review |
| `overview.chip_idle` | No card |
| `overview.held` | Held for {duration} |
| `overview.submitted` | Submitted {duration} ago |
| `overview.idle` | Nothing drawn right now |
| `boards.heading` | Watched boards |
| `boards.meta` | Board {id} · watching since {date} |
| `boards.remove` | Remove |
| `boards.add_heading` | Watch another board |
| `boards.add_label` | Board ID or URL |
| `boards.add_hint` | The board needs the six lanes: Proposed, Augmented, Ready, In Progress, In Review, Done. |
| `boards.add_button` | Watch board |
| `boards.err_not_found` | No board with ID {id}. Check the ID, or paste the board's URL from Vikunja. |
| `boards.err_lanes` | Watching needs six lanes with these exact names. This board is missing {count}: |
| `boards.err_lanes_tail` | Add the struck-through lanes in Vikunja, then try again. |
| `boards.err_watched` | "{name}" (board {id}) is already on the watch list. Nothing to change. |
| `kids.heading` | Kids |
| `kids.meta` | Added {date} |
| `kids.show_qr` | Show QR |
| `kids.regenerate` | Regenerate |
| `kids.add_heading` | Add a kid |
| `kids.add_label` | Name |
| `kids.add_hint` | This makes the kid's personal link and QR code. The link is the whole login - anyone who has it sees that kid's card. |
| `kids.add_button` | Add kid |
| `qr.print` | Print |
| `qr.back` | Back to kids |
| `qr.warn_heading` | Regenerating replaces the link |
| `qr.warn_body` | If this link is lost or shared too widely, regenerate it from the kids list. The old link stops working the moment the new one is made - re-print or re-scan the new QR after. |

**Total: 30 strings** (all adult-facing; no reading-level constraint
applies on this surface).

## Review ledger

Gate A findings and dispositions land here.

- 2026-08-18 Gate A round 1: FAIL with one MINOR finding. Author:
  kimi-for-coding/k3 (board owner); reviewer: openai/gpt-5.5 via the
  general role (ses_fe8ccd8b8ffe6LQ0NQNTCCtS87), fresh context,
  pre-vetted by nonce readback (timber-owl-5520). The reviewer
  differs in family from the author.
  1. MINOR the five pages wrapped content in a plain div, not a main
     landmark - ACCEPTED. Fixed in 5a8594f (wrapper tag swap only, no
     visual change).
- 2026-08-18 Gate A round 2: PASS, the round-1 finding verified
  RESOLVED, no new issues. Reviewer: openai/gpt-5.5 via the general
  role (ses_fe8caa0f6ffeO0PuHPiiH9n87l), fresh context.
