# AGENTS.md

## Git

- Org-level settings need the `admin:org` token scope. The default `gh` token does
  not have it. `gh auth refresh -h github.com -s admin:org` is interactive, so ask
  the user to run it rather than attempting it.

## profile/README.md

- **Write each paragraph on a single line.** GitHub renders README newlines as
  whitespace, but the `/markdown` API's `gfm` mode inserts hard breaks. Unwrapped
  source makes the file and every renderer agree.
- **Images need absolute `raw.githubusercontent.com` URLs pinned to `main`.**
  Relative paths are unreliable in org profile READMEs. Images 404 until the change
  reaches `main`; that is expected, not a bug.
- Alt text must carry whatever the image says, since the banner holds the headline.
- No shields.io badges. No `<div align="center">`. No `<style>`, GitHub strips it.

## profile/assets/

Every file here is exported from the Paper file **Studio Jocks UG**, page
**GitHub kit**. Do not hand-edit an export. Change the artboard in Paper and
export again. Renaming or moving a file breaks the live profile immediately.

Read `README.md` before changing any image or link. It records the exceptions to
that pipeline and the limits of GitHub's markdown renderer: stripped `tel:` URLs,
auto-linked bare images, and `<picture>` being hoisted out of anchors. Each of
these will silently break something if rediscovered the hard way.

## Verify rendering before pushing

No local markdown preview matches GitHub. Render through GitHub's own API instead:

```bash
python3 -c 'import json;print(json.dumps({"mode":"gfm","text":open("profile/README.md").read()}))' > /tmp/b.json
gh api --method POST /markdown --input /tmp/b.json > /tmp/body.html
```

Wrap `/tmp/body.html` in `github-markdown-css` at `max-width: 890px`, the profile's
real content width, and screenshot it with headless Chrome. Check both colour
schemes: `--blink-settings=preferredColorScheme=0` is dark, `=1` is light. The
`<picture>` theme switch only resolves correctly when that flag is set.

Neither API mode matches GitHub's README pipeline exactly: `gfm` renders alerts but
adds hard breaks, `markdown` does the reverse. After pushing, confirm against the
live page rather than trusting the render.
