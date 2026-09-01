# RUOOD Lab Announcements

Remote announcement data for RUOOD Lab. **Published by the Announcement
Manager — do not edit `dist/` by hand.**

```
content/                    source of truth, including drafts and archives
  announcements/<id>.json   one file per record
  media/<id>.<ext>          image originals, full resolution
  retired-ids.json          ids that can never be reused
  state.json                the revision counter
dist/                       BUILT — the only thing the app reads
  announcements.json
  images/<id>-<hash8>.webp  content-addressed, cacheable for ever
```

## For anyone who finds this repository

The app reads `dist/announcements.json` and nothing else. If it is missing,
malformed, or unreachable, **RUOOD Lab carries on working normally** — an
announcement is never a dependency for the app starting.

`paused: true` at the root of that file is a kill switch: every install shows
nothing until it goes back to `false`.

## Signing

`dist/announcements.json` is signed with Ed25519 over the canonical form of the
whole file — including `paused`, `revision` and which records are present. The
public key is in `keys/announcement-signing.pub` and is committed on purpose; a
public key is public. The private key lives outside every repository.

CI re-checks the signature on every push (`.github/workflows/verify.yml`). If it
fails, `dist/` was edited by something other than the Manager.

## Channels

`dist/announcements.json` is production. `dist/staging/announcements.json` is
the staging channel, which dev builds read and which additionally carries
**drafts** — that is what it is for. Each is published by its own commit.

## Ids are permanent

An id is never reused, including after its record is deleted, because it keys
impression state on every device that ever saw it. `content/retired-ids.json`
is the ledger of ids that are gone for good.
