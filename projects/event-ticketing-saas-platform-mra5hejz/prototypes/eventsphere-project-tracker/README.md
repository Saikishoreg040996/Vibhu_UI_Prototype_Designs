# EventSphere · Pre-Go-Live Tracker

Single-file status board for the beta launch. 123 modules across six surfaces, each tracked
independently for **backend** and **frontend**.

## Deploy

This tracker ships inside the designs repo, in a sub-folder — it is **not** at the repo root.
Pages already serves it at
`https://designs.vibhusolutions.com/projects/event-ticketing-saas-platform-mra5hejz/prototypes/eventsphere-project-tracker/`.

## Configuration

Owner, repo, branch and path are **baked into `index.html`** (the `REPO` constant) and match
`git remote`. There is nothing to fill in — the page pulls live status on load. A browser that
still has the old root-level `status.json` path saved in localStorage is migrated automatically.

The only thing not in the repo is the **token**, and that is deliberate: this folder is served
publicly from GitHub Pages, so a PAT committed into `index.html` would be readable by anyone who
opens the page. GitHub also scans public pushes and auto-revokes tokens it finds.

### Supplying the token

**Local development** — create `tracker.local.json` next to `index.html`:

```json
{ "token": "github_pat_...", "who": "your-name" }
```

It is covered by the root `.gitignore`, and is read **only** when the page is served from
`localhost` / `127.0.0.1`, held in memory, never written to localStorage. Serve over `http://`,
not `file://` — a `file://` origin blocks both `localStorage` and the boot fetch.

**Published site** — click **Repo** once and paste your own PAT. It stays in that browser's
localStorage. Each engineer uses their own token, scoped `Contents: Read and write` on this
repo only.

Serve over `http://` — not `file://`. Opening the page from disk gives it an opaque origin, so
`localStorage` (where the config lives) and the `./status.json` boot read are both blocked.

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
