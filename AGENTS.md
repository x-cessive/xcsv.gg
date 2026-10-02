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
