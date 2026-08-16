# jocks-studio/.github

GitHub's special repository for the `jocks-studio` organisation. It holds the org
profile and, later, org-wide community health defaults.

`profile/README.md` is not a normal repository README — it renders at
<https://github.com/jocks-studio>. Its images use absolute
`raw.githubusercontent.com` URLs pointing at the **default branch**, because
relative paths in an org profile README are not reliably resolved. Two
consequences: images stay broken until the change reaches `main`, and renaming
anything under `profile/assets/` breaks the live profile.

## Assets

Everything in `profile/assets/` comes from the Paper file **Studio Jocks UG**, page
**GitHub kit**, where artboard and layer names match the filenames. Do not
hand-edit an export — change it in Paper and export again. Banners and buttons are
exported at 2x; buttons are displayed at `height="40"`.

Two things do not follow that pipeline:

- **Banners are PNG, not SVG.** GitHub's markdown pipeline does not embed
  webfonts, so text-bearing vector art would fall back to another face and lose
  Helvetica Neue.
- **The icon SVGs are hand-authored** from the artboard geometry. Paper's SVG
  export wraps the design in a `foreignObject`, which GitHub strips and which
  weighs 2.4 MB. The icon is five rectangles on a 512 grid; if it changes in
  Paper, read the children's computed styles and update the `<rect>` coordinates.

`avatar-512.png` is used by no page. It is the file to upload manually under
Organisation settings → Profile picture; GitHub cannot read an avatar from a
repository.

## GitHub's markdown limits

Verified against the live rendering pipeline. These are why the contact buttons
are built the way they are:

- **`tel:` links are stripped**, in HTML anchors and markdown links alike. Only
  `https:` and `mailto:` survive. That is why the call button is a Cal.com
  scheduling link rather than a dial link — a `tel:` button would render as a
  dead image.
- **A bare image is auto-wrapped in a link to its own raw file.** Any button
  image must sit inside an `<a>`, or clicking it opens the PNG.
- **`<picture>` cannot go inside a link.** GitHub rewrites it to
  `<themed-picture>` and hoists it out of the anchor, silently dropping the link.
  Buttons are therefore single assets that must read on white and on GitHub dark
  alike.

## Brand constraints

The profile follows the identity manual, not GitHub convention:

- Two colours and a neutral scale. Ink `#141719`, Signal `#E8481C`, Paper
  `#F4F4F2`. One accent per surface, never two — which is why there are no
  shields.io badges.
- No emoji, no exclamation marks, no centred layout.
- Keep the page short. A section that does not earn its place comes out.

## Not yet present

Org-wide community health defaults — `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
`SECURITY.md`, `SUPPORT.md`, issue templates, pull request template — are
deliberately absent. They were scoped out of the profile setup, not forgotten.
Adding them here makes them inherit into every `jocks-studio` repository that does
not define its own.
