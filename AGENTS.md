# AGENTS.md

Instructions for coding agents working in `jocks-studio/.github`, the GitHub
organisation profile repository for Studio Jocks. `README.md` describes what the
repo contains.

Claude Code reads `CLAUDE.md`, which imports this file. Edit this one; leave
`CLAUDE.md` as the import.

There is no build, no test suite and no dependencies here. The deliverable is
rendered markdown and static image assets.

## Commits

Use [Conventional Commits](https://www.conventionalcommits.org/): `type(scope): subject`.

- Types in use: `feat`, `fix`, `refactor`, `docs`, `chore`.
- Scope is the area touched — usually `profile` or `assets`. Omit it for
  repo-wide changes.
- Subject in the imperative mood, lower case, no trailing period.
- Explain *why* in the body when the diff does not make it obvious. Skip the body
  when the subject already says everything.

**Commit in small, meaningful pieces.** One logical change per commit. Do not
bundle a copy rewrite, an asset swap and a settings change together. If the body
needs a bulleted list of unrelated changes, that is two or more commits.

### Never credit a model

**Do not add `Co-Authored-By` trailers naming Claude, any Anthropic model, or any
other AI tool.** No "Generated with Claude Code" lines either. This applies to
commit messages, PR titles and bodies, tags, and release notes. Commits are
authored by the repository owner.

## Git

- `main` is the default branch, protected by the `main protection` ruleset:
  force-pushes and branch deletion are blocked, and **commits must be signed**.
  SSH signing is already configured — do not disable it.
- Branch off `main` for anything non-trivial; delete the branch after merging.
- Do not push, open a PR, or change repository settings unless asked.
- Org-level settings need the `admin:org` token scope. The default `gh` token does
  not have it — `gh auth refresh -h github.com -s admin:org` is interactive, so ask
  the user to run it rather than attempting it.

## profile/README.md

This file renders at <https://github.com/jocks-studio>. It is the organisation's
public face, not a normal repo README. Treat copy changes as design work.

- **Write each paragraph on a single line.** GitHub renders README newlines as
  whitespace, but the `/markdown` API's `gfm` mode inserts hard breaks. Unwrapped
  source makes the file and every renderer agree.
- **Images need absolute `raw.githubusercontent.com` URLs pinned to `main`.**
  Relative paths are unreliable in org profile READMEs. Images 404 until the change
  reaches `main`; that is expected, not a bug.
- Alt text must carry whatever the image says, since the banner holds the headline.
- No emoji. No shields.io badges. No `<div align="center">`. No `<style>` — GitHub
  strips it.
- Sentence-case headings. Keep the page short; every section must earn its place.
- Voice: concrete nouns, no marketing register, no exclamation marks. State a
  measure where there is one. If a claim cannot be supported, cut it rather than
  soften it.

## profile/assets/

Every file here is exported from the Paper file **Studio Jocks UG**, page
**GitHub kit**. Do not hand-edit an export — change the artboard in Paper and
export again. Renaming or moving a file breaks the live profile immediately.

Paper's SVG export wraps output in a `foreignObject`, which GitHub strips and which
runs to megabytes. The icon SVGs are therefore hand-written from the artboard
geometry; if the icon changes in Paper, read the children's computed styles and
update the `<rect>` coordinates to match.

## Verify rendering before pushing

No local markdown preview matches GitHub. Render through GitHub's own API instead:

```bash
python3 -c 'import json;print(json.dumps({"mode":"gfm","text":open("profile/README.md").read()}))' > /tmp/b.json
gh api --method POST /markdown --input /tmp/b.json > /tmp/body.html
```

Wrap `/tmp/body.html` in `github-markdown-css` at `max-width: 890px` — the profile's
real content width — and screenshot it with headless Chrome. Check both colour
schemes: `--blink-settings=preferredColorScheme=0` is dark, `=1` is light. The
`<picture>` theme switch only resolves correctly when that flag is set.

Neither API mode matches GitHub's README pipeline exactly: `gfm` renders alerts but
adds hard breaks, `markdown` does the reverse. After pushing, confirm against the
live page rather than trusting the render.

## Maintaining this file

Keep it under 200 lines and prune anything that has gone stale — it loads into every
agent session, so length costs context and reduces adherence. Do not restate what
`README.md` already documents, and do not add rules the branch ruleset already
enforces.
