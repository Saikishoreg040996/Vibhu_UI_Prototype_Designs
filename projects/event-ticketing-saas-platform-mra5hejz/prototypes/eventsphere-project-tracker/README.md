# EventSphere · Pre-Go-Live Tracker

Single-file status board for the beta launch. 123 modules across six surfaces, each tracked
independently for **backend** and **frontend**.

## Deploy

1. Push `index.html` + `status.json` to a repo.
2. Settings → Pages → Source: `main` / root.
3. Open `https://<owner>.github.io/<repo>/`.

## Read vs write

Anyone with the URL sees live status — no token, no login. Status is read from `status.json`
in the repo.

To **update**, an engineer clicks **Repo** once and pastes a GitHub fine-grained PAT scoped to
this repo only, with `Contents: Read and write`. The token lives in that browser's
localStorage and is never committed. **Push** writes `status.json` back as a normal commit,
so every status change is a reviewable entry in the repo history.

## Conflict handling

Writes are `sha`-guarded. If someone else pushed since your last pull, GitHub returns 409 and
the footer tells you to pull, re-apply, push. No silent overwrites.

## Statuses

`NS` not started · `WIP` in progress · `QA` in test · `DONE` shipped · `BLK` blocked

A blocked track reveals a reason field on the row. Blocked rows get a red gutter and are
filterable from the header.

## Offline / air-gapped

**Export** downloads `status.json`; **Import** pastes one back. Commit the file manually if
you'd rather not hand out tokens at all.
