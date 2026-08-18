# Kid PWA Copy Deck

Complete user-facing strings for the six kid PWA states. Each string has a stable key for reference in mockups and implementation.

## 0. Common

Strings shared across states.

| Key | Text | Notes |
|-----|------|-------|
| `common.busy` | One moment... | Shown while a draw or photo submit is in flight |

## 1. Card View

The current chore: title, location, steps, optional video, checklist graphic, due date.

| Key | Text | Notes |
|-----|------|-------|
| `card.heading` | Your chore | Page header |
| `card.title_label` | **{title}** | Chore title (bold, large) |
| `card.location_label` | In the {location} | e.g., "In the kitchen", "In your room" |
| `card.steps_heading` | What to do | Section heading |
| `card.video_label` | Watch how | Link label when a video exists |
| `card.checklist_alt` | Steps to check off | Alt text for checklist graphic |
| `card.due_label` | Finish by {date} | Only if the card has a due date |
| `card.primary_button` | Done | Main action button |
| `waiting.heading` | All sent! | Shown after photo submit, in place of the Done action |
| `waiting.body` | {ParentName} will check your photos. | Under the disabled stamp; no parent has acted yet, so a single mapped name is used, otherwise "Your parent will check your photos." |

## 2. Done Flow

What the kid sees when they tap Done (before photo upload).

| Key | Text | Notes |
|-----|------|-------|
| `done.heading` | Mark it done? | Modal or screen heading |
| `done.body` | Take a photo to show the work. | Explanation text |
| `done.primary_button` | Add photos | Primary action |
| `done.secondary_button` | Cancel | Dismiss button |

## 3. Photo Upload

Add photo(s); photos are required to finish; up to 10.

| Key | Text | Notes |
|-----|------|-------|
| `upload.heading` | Add photos | Page header |
| `upload.helper` | Take photos of your work. Add 1-10 photos. | Instructional text |
| `upload.add_button` | Add more | Button to add additional photos |
| `upload.remove_button` | ✕ | Remove photo button (icon) |
| `upload.finish_button` | Finish | Submit button |
| `upload.back_button` | Back | Cancel/back button |
| `upload.error_no_photos` | Add at least one photo. | Validation error |
| `upload.error_too_many` | Too many photos. The limit is 10. | Validation error |
| `upload.photo_count` | {count}/10 photos | Progress indicator |
| `upload.photo_alt` | Photo of finished work | Alt text for thumbnails |

## 4. Rejection with Notes

Parent rejected the work; notes shown plainly.

| Key | Text | Notes |
|-----|------|-------|
| `reject.heading` | Not done yet | Page header |
| `reject.body` | {ParentName} says it needs more work. | e.g., "Dad says it needs more work.", "Mom says it needs more work." |
| `reject.notes_label` | Notes | Section heading for parent's notes |
| `reject.notes_content` | **{notes}** | Parent's notes (bold) |
| `reject.primary_button` | Try again | Main action button; returns to the card view so the kid can redo the work and resubmit |

## 5. Empty State

No current assignment: the kid opened the app and holds no chore (never drew, or the last one was approved).

| Key | Text | Notes |
|-----|------|-------|
| `empty.heading` | Nothing to do yet | Page header |
| `empty.body` | Tap the button to get your chore. | The QR code IS the URL; there is no scan step inside the app |
| `empty.button` | Get my chore | Primary action: triggers the draw |

## 6. Returned to Pool / All Caught Up

Two related moments: work approved (or a drawn chore returned to the pool by a parent), and a draw that found no Ready cards.

| Key | Text | Notes |
|-----|------|-------|
| `returned.heading` | Nice work! | Page header after approval |
| `returned.body` | {ParentName} says it is done. | e.g., "Dad says it is done." |
| `returned.body_returned` | That chore went back to the pile. | When a parent returns the drawn chore to the pool |
| `returned.button` | Get my chore | Draw again |
| `caughtup.heading` | All caught up | Page header when a draw finds no Ready cards |
| `caughtup.body` | No chores right now. Check back later. | Explanation text |
| `caughtup.button` | Check again | Refresh/retry button |

## Narrative Prose for Mockup Index Doc

This prose describes each state for a parent or developer reader.

### Card View

The kid sees their one assigned chore. A large title sits at the top, followed by the location in the house. Below that, step‑by‑step instructions appear as plain bullet points or a checklist graphic. An optional how‑to video link sits under the steps. If the chore has a due date, it shows in a clear line above the Done button. The screen shows exactly one thing: this chore, with a single Done button at the bottom. After photo submit the Done button turns into a disabled stamp. "All sent!" appears above the stamp and "{ParentName} will check your photos." below it; the card stays up while a parent checks.

### Done Flow

Tapping Done opens a confirmation screen or modal. The text “Take a photo to show the work” explains the requirement. Two buttons appear: Add photos proceeds to the upload screen; Cancel returns to the chore view. The flow is a single interruption, not a multi‑step wizard.

### Photo Upload

A grid of photo thumbnails fills the screen. The helper text “Take photos of your work. Add 1-10 photos.” sits near the top. An Add more button lets the kid capture additional photos up to the limit of ten. Each thumbnail has a remove button (×). A Finish button submits the photos; Back cancels and returns to the chore view. Validation errors appear inline if the kid tries to finish with zero photos or more than ten.

### Rejection with Notes

When a parent rejects the submitted work, the kid sees a plain screen headed "Not done yet." The rejection uses the parent's name: "Dad says it needs more work." or "Mom says it needs more work." Below that, a Notes section shows the parent's feedback in bold. A Try again button returns the kid to the card view so they can redo the work and resubmit.

### Empty State

Before the first draw, or after a completed chore with no immediate next draw, the kid sees "Nothing to do yet" with a short instruction: "Tap the button to get your chore." The button triggers the draw. The QR code is the URL itself, so there is no scan step inside the app. This screen is the default landing state when no assignment exists.

### Returned to Pool / All Caught Up

After approval, the kid sees "Nice work!" with the parent's name: "Dad says it is done." A Get my chore button draws the next card. When a parent returns a drawn chore to the pool, the body reads "That chore went back to the pile." with the same draw button. When a draw finds no Ready cards on any watched board, the kid instead sees "All caught up" with "No chores right now. Check back later." and a Check again button. The caught-up state is distinct: it follows a draw attempt that returned the typed `NoChoresAvailable` error, not the absence of any assignment.

## Board-Owner Amendments (2026-08-14)

Decisions on the open questions from the copy-deck dispatch:

1. The remove control keeps the "✕" glyph and gains an `aria-label` of "Remove photo"; the tap target stays large.
2. "Ask for help" is dropped. No notification path exists yet (that is S15 scope), and the card names only notes plus retry for the rejection view.
3. A `common.busy` string was added for draw and submit transitions.
4. The empty-state scan copy was replaced: the QR code is the URL, so the in-app action is the draw itself ("Get my chore").
5. State 6 gained explicit `returned.*` strings for the post-approval and parent-returned moments alongside the caught-up variant.

## Gate-Approved Amendments (2026-08-18)

Approved by the user at S8's U(code-review) gate:

1. `waiting.heading` and `waiting.body` were added for the after-submit moment, which the original deck had no copy for. The body names a parent only when the deployment maps exactly one (`CHORE_KID_PARENT_NAMES`); with zero or several mapped names it falls back to "Your parent will check your photos.", the same fallback as the reject/approve copy.

## String Count Summary

- Card View: 12 strings
- Done Flow: 4 strings
- Photo Upload: 12 strings
- Rejection with Notes: 6 strings
- Empty State: 3 strings
- Caught Up: 3 strings

**Total: 40 user‑facing strings**