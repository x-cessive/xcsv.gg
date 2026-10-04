# AGENTS.md — xcsv.gg

Static marketing site for XCSV. `index.html` + `assets/`, no build step.
Brand law lives in `x-cessive/xcsv-discord-management` → `docs/BRAND.md`
(amber `#e8a13d`, navy `#1f3a5f`, off-white `#f5f0e6` only; solid colors, pixel art, no gradients).

## Deploy

SITE DEPLOY RULE: site work is not done until it's live on https://xcsv.sovranos.com. Every site task ends with: merge to main → wrangler deploy to the xcsv-gg Pages project → verify the live URL returns 200 with the new content. A merged PR with no deploy is an unfinished task. (If the Pages project ever gets connected to GitHub, pushes to main auto-deploy and the manual step goes away.)

How (the Pages project `xcsv-gg` is direct-upload, not Git-connected):

```sh
# deploy a clean export of main so local junk (.wrangler/, .git/) is never uploaded
rm -rf /tmp/xcsv-deploy && mkdir -p /tmp/xcsv-deploy
git archive main | tar -x -C /tmp/xcsv-deploy && rm -f /tmp/xcsv-deploy/.gitignore /tmp/xcsv-deploy/AGENTS.md
npx wrangler pages deploy /tmp/xcsv-deploy --project-name xcsv-gg --branch main \
  --commit-hash "$(git rev-parse --short main)"
curl -s -o /dev/null -w "%{http_code}\n" https://xcsv.sovranos.com/
```

Preview a branch without touching production: same command with `--branch <branch>`;
it serves at `https://<branch>.xcsv-gg.pages.dev`.

## AI Handoff & Desktop Logging Rule

ALWAYS update the desktop log upon completing a session, milestone, or handoff:
1. Save the release report or progress summary in `C:\Users\justi\Desktop\XCSV\`.
2. Always maintain `C:\Users\justi\Desktop\XCSV\ai.md` with:
   - Current live state, commit hash, and repo paths.
   - Pending decisions for Justin.
   - Active backlog and exact next tasks for the next AI session to pick up immediately.

---

## UI/UX design

Full library: https://raw.githubusercontent.com/x-cessive/design-library/main/DESIGN_LIBRARY.md

**The rule.** AI output is bounded by reference quality. Never design a UI
from a blank page:
1. Pick 2-3 references from the library before building (galleries for the
   visual bar, pattern libraries for the specific elements).
2. Describe with patterns, not adjectives - link the pattern.
3. If this repo has a design-system doc, it is the source of truth and wins
   over generic references.

**Brand for this repo.** Unknown — propose brand tokens on the next UI change and get them reviewed. Don't invent silently.

**Scope.** Applies to every user-facing surface built from this repo: app
UI, web pages, generated docs/presentations, screenshots. Don't invent
brand silently - propose it in the change and get it reviewed.

---

## Release management

**The rule.** One repo, one canonical checkout per build machine. Stale
duplicate checkouts are how an AI ends up building the wrong thing.

- Canonical checkout on the build machine: TBD — do not assume; if multiple checkouts exist, ask the operator which is canonical.
- Never create suffixed duplicate clones (`-old`, `-backup`, `-copy`,
  `-2`, `-final`, etc.). Scratch copies get deleted when done.
- If you discover multiple checkouts of this repo on the build machine,
  STOP and ask the operator which is canonical. Do not guess.
- Build outputs are versioned artifacts. After a new build is verified
  and pushed, archive superseded builds: move them to
  `_archive/<name>-<date>/`, keep the last 2, older ones may be deleted.
- This section governs hygiene. The repo's own build docs are the
  authority on where artifacts go.
