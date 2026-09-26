# The Emperor's Expendables — Campaign Wiki

A campaign wiki for our SWRPG game: session recaps, a campaign-wide timeline,
open threads and a cross-linked cast.

## Where the content comes from

Sessions 1–3 were compiled from the GM's campaign ledger (kept in a separate
private repo). Everything here is **player-safe**: only what the party has
learned at the table. Future sessions will be added from recordings:

```
Craig recording  ->  faster-whisper  ->  transcript.txt  ->  content/*.md  ->  docs/index.html
```

`content/` is an [Obsidian](https://obsidian.md) vault. Every entity —
character, place, ship, item, faction — is one markdown file with
`[[wikilinks]]` to the others. Open the folder in Obsidian and you get local
editing, backlinks and the graph view for free.

`build.py` turns that vault into a single self-contained HTML page in
`docs/`. Standard library only, no dependencies:

```bash
python build.py
```

It fails loudly on any `[[wikilink]]` that points at a page which doesn't
exist, and renders those links in red on the page, so a typo can't silently
become a missing page.

**The generated site is committed to `docs/`.** If you edit anything in
`content/`, run `build.py` before committing or the published page won't
change.

## Campaign settings

Everything campaign-specific — name, sidebar tagline, meta description and
the footer's source note — lives in the `CAMPAIGN` block at the top of
`build.py`. `template.html` carries no campaign names. The sidebar's
"Session N" is worked out from the highest `session:` in the vault.

## Adding a session

1. Record with Craig, download the **multi-track** version.
2. Copy `names.txt` next to the tracks, transcribe, merge by timestamp.
3. Add `content/session-N.md` (`group: "Sessions"`, `session: N`, and an
   `order` that puts it first), append a `## Session N` block to
   `timeline.md`, and update `threads.md` and `overview.md`.
4. Add or update entity pages.
5. `python build.py`, then commit and push.

## Frontmatter

```yaml
---
title: R6-K4 "Rusk"
group: "Vannix"             # sidebar section (see GROUP_ORDER in build.py)
type: character             # character | place | ship | thing | faction | doc
conf: hi                    # hi | mid | lo  — how confident we are
short: "Rusk"               # optional: the form people actually say
aka: ["R6-K4"]              # real alternate names
aliases: ["Rusque"]         # transcription mangles
session: 3                  # session pages only
dek: "One line under the title."
---
```

`aka` and `aliases` both make `[[links]]` resolve, but they are different
things:

- **`aka`** — genuine alternate names (`R6-K4` for Rusk, `Kennaud Thorn` for
  Kenno). Shown as *Also known as*, added to `names.txt`, never to
  `corrections.json`.
- **`aliases`** — spellings a transcript actually produced. Shown as *Also
  transcribed as*, and they feed `corrections.json`.

Mixing them up would have `corrections.json` "repairing" a real name.

Session docs use `type: doc`; the timeline and threads pages use
`layout: timeline` / `layout: threads`. In the timeline, `## Session N` lines
become dividers, and each beat is ``- `M02` **Title** — text``. The marker is
the mission (`M01`, `M02`, `INT` for interludes), not a timestamp.

## The two name files

`build.py` regenerates both on every run. They do opposite jobs:

- **`names.txt`** — canonical spellings only. Prompted *into* the
  transcriber so it biases towards the right words.
- **`corrections.json`** — alias → spoken form. Applied to the transcript
  *after* recognition, to repair what the prompt didn't prevent.

## What's deliberately not here

GM notes, stat blocks, NPC secrets, plot the players haven't discovered, and
anything said at the table that wasn't in character. This repository is
public; that material stays in the private ledger repo or locally in `gm/`,
which `.gitignore` excludes.
