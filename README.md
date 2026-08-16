# jocks-studio/.github

GitHub's special repository for the `jocks-studio` organisation. It holds the
org profile and, later, org-wide community health defaults.

## What renders where

| Path | Renders at |
| --- | --- |
| `profile/README.md` | <https://github.com/jocks-studio> — the organisation profile |
| `README.md` (this file) | Only on this repository's own page |

`profile/README.md` is not a normal repository README. Its images use absolute
`raw.githubusercontent.com` URLs pointing at the **default branch**, because
relative paths in an org profile README are not reliably resolved. Two
consequences:

- Images stay broken until the change is merged into `main`.
- Renaming or moving anything in `profile/assets/` breaks the live profile.

## Assets

Everything in `profile/assets/` is exported from the Paper file **Studio Jocks
UG**, page **GitHub kit**. Do not hand-edit an export — change the artboard in
Paper and export again.

| File | Source artboard | Export setting |
| --- | --- | --- |
| `banner-dark.png` | `GH 01 · Profile banner — ink` | PNG `2x` → 2560 × 640 |
| `banner-light.png` | `GH 02 · Profile banner — paper` | PNG `2x` → 2560 × 640 |
| `avatar-512.png` | `GH 03 · Org avatar 512` | PNG `1x` → 512 × 512 |
| `icon-positive.svg` | `GH 04 · Icon on paper 512` | hand-authored, see below |
| `icon-reversed.svg` | `GH 03 · Org avatar 512` | hand-authored, see below |
| `btn-email.png` | `GH 05 · Contact buttons` → `btn-email` | PNG `2x`, displayed at `height="40"` |
| `btn-call.png` | `GH 05 · Contact buttons` → `btn-call` | PNG `2x`, displayed at `height="40"` |
| `btn-whatsapp.png` | `GH 05 · Contact buttons` → `btn-whatsapp` | PNG `2x`, displayed at `height="40"` |

The banners ship as PNG rather than SVG on purpose: GitHub's markdown pipeline
does not embed webfonts, so text-bearing vector art would render in a fallback
face and lose Helvetica Neue.

The two icon SVGs are **hand-authored from the artboard geometry**, not exported.
Paper's SVG export wraps the design in a `foreignObject`, which GitHub strips and
which weighs 2.4 MB. The icon is five rectangles on a 512 grid, so the file is
transcribed by hand instead — if the icon changes in Paper, read the children's
computed styles and update the `<rect>` coordinates to match.

### Contact buttons

Three constraints shape how these are built, all verified against GitHub's
markdown pipeline:

- **`tel:` links are stripped**, in HTML anchors and markdown links alike. Only
  `https:` and `mailto:` survive. The call button therefore carries the number on
  its face instead of being a dial link, and points at `#contact` — a bare image
  gets auto-wrapped in a link to the raw PNG, which is worse.
- **`<picture>` cannot go inside a link.** GitHub rewrites it to
  `<themed-picture>` and hoists it out of the anchor, silently dropping the link.
  So the buttons are single assets, not theme-switched pairs.
- Because there is no theme switch, each button must read on white *and* on
  GitHub's dark background: the accent button is filled, the other two are white
  with a 2px ink border.

`avatar-512.png` is **not** used by any page. It is the file to upload manually
under Organisation settings → Profile picture; GitHub cannot read an avatar from
a repository.

## Brand constraints

The profile follows the identity manual, not GitHub convention:

- Two colours and a neutral scale. Ink `#141719`, Signal `#E8481C`, Paper
  `#F4F4F2`. One accent per surface, never two — which is why there are no
  shields.io badges.
- No emoji, no exclamation marks, no centred layout.
- Every claim links to a source. If a source goes offline, say so in the text
  rather than leaving a dead link.

## Not yet present

Org-wide community health defaults — `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
`SECURITY.md`, `SUPPORT.md`, issue templates, pull request template — are
deliberately absent. They were scoped out of the profile setup, not forgotten.
Adding them here makes them inherit into every `jocks-studio` repository that
does not define its own.
