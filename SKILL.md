---
name: pr-description-writer
description: Writes pull request descriptions that reviewers and non-engineers can both use - a plain-language business summary on top, then scope, motivation, how it was tested, risk and rollback, a screenshots checklist for UI changes, and specific guidance on where reviewers should look. Built from the diff, commit list and linked issue. Use when opening a PR, when a PR body is empty or says only "fixes bug", or when a change needs sign-off from product or operations.
---

# PR Description Writer

A PR description is the reviewer's map and the future reader's explanation. It should let a reviewer start in the right file and let a manager understand the change without opening the diff.

## When to use
- Opening a new pull or merge request.
- A PR body is empty, out of date, or just a ticket link.
- The change touches money, permissions, data migrations or user-visible UI and needs a clear risk statement.

## Inputs
Read the diff (or `git diff <base>...HEAD`), the commit messages, and the linked issue. Ask the author for anything the diff cannot show: why now, what was tested manually, deploy order constraints.

## Method
1. **Summarise in business language (2-3 sentences).** Who benefits and how, no code terms. "Finance users can now export invoices to Excel. Previously they had to copy rows by hand."
2. **Scope.** Bullet what changed, grouped by area (API, UI, database, config). Add an explicit **Out of scope** line for related things deliberately not done.
3. **Why.** The problem, link to the issue, and the reason for this approach over the obvious alternative (one line each).
4. **How tested.** Automated tests added or changed (name the files or cases), manual steps a reviewer can repeat, environments used. Say what was **not** tested.
5. **Risk and rollback.** Rate Low / Medium / High with the reason. State rollback: plain revert, feature flag, or a down-migration. Flag irreversible steps (data backfills, deleted columns, sent emails).
6. **Screenshots checklist** (UI changes only): before/after, both themes if supported, narrow mobile width, right-to-left layout if the product supports Arabic, empty and error states.
7. **Reviewer guidance.** Where to start, which files are mechanical (renames, generated code) and can be skimmed, and the one or two places you most want scrutiny.
8. **Deploy notes.** Ordering (migrate before deploy), new configuration keys (names only, never values), feature flags, follow-up PRs.

## Template
```
## Summary
<2-3 sentences, business language>

## What changed
- ...
**Out of scope:** ...

## Why
Closes #<issue>. ...

## How tested
- [ ] Unit: ...
- [ ] Integration/e2e: ...
- [ ] Manual: 1. ... 2. ...
Not tested: ...

## Risk & rollback
Risk: Low | Medium | High - because ...
Rollback: ...

## Screenshots (UI only)
- [ ] Before / after
- [ ] Mobile width
- [ ] Dark mode
- [ ] RTL / Arabic
- [ ] Empty + error states

## Reviewer guide
Start at ... Skim ... Look hard at ...

## Deploy notes
...
```
Remove sections that genuinely do not apply rather than writing "N/A" everywhere - but never drop Risk & rollback.

## Worked example (abridged)
Diff: adds a `discount_cap` column, validates it in the checkout service, shows it in the admin form.
- Summary: "Admins can set a maximum discount per coupon, so a misconfigured coupon can no longer give away more than intended."
- Risk: Medium - touches checkout totals. Rollback: revert; the new column is nullable and ignored by old code.
- Reviewer guide: "Start at `checkout/totals` - the rounding order changed. The admin form is mostly generated."

## Pitfalls
- Restating the diff line by line instead of explaining intent.
- "Tested locally" with no steps.
- Hiding a migration or config change deep in the list.
- Pasting secrets, customer data or real tokens into screenshots or logs - redact them.
- Letting the description go stale after review changes; update it before merge.
