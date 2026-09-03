# REPO — Build & Deploy Mirror (GitHub Pages)

> ⚠️ **This repository contains no source code.**
> It exists solely to build the ProperDocs site hosted on Codeberg and
> publish the compiled output to GitHub Pages.

## Why this repo exists

The canonical source for this site lives on Codeberg:

**https://codeberg.org/inigochoa/server-inigochoa-me**

GitHub Pages is used purely as a build-and-serve layer. The source code
is **never committed here** — it only passes through the Actions runner
ephemerally during the build and is discarded once the job finishes.

## How it works

1. **Develop locally** and push commits to the source repository on
   Codeberg, as usual.
2. **Pushing to Codeberg does not publish anything.** This repo has no
   automatic triggers (no `push`, no `schedule`, no webhooks).
3. **To publish**, go to the **Actions** tab in this GitHub repository and
   manually run the *Build and Deploy* workflow.
4. The workflow then:
   - Clones the latest commit from the Codeberg source repo into `./site`
     (the source dir properdocs uses as build output is `site/`, so the
     clone is not used as a workspace — see structure below).
   - Sets up Python and installs the dependencies from `requirements.txt`.
   - Builds the site with `properdocs build`.
   - Uploads the built `site/` directory as a Pages artifact and deploys it
     to GitHub Pages.

No source code or intermediate files are ever committed to this
repository — everything happens transiently inside the workflow run.

## Publishing a new version

1. Make sure your changes are committed and pushed to Codeberg
   (`https://codeberg.org/inigochoa/server-inigochoa-me`).
2. Open this repository on GitHub.
3. Go to the **Actions** tab.
4. Select **Build and Deploy** in the left sidebar.
5. Click **Run workflow** (top right), confirm the branch, and click
   **Run workflow** again.
6. Wait for the `build` and `deploy` jobs to finish. The live URL is shown
   in the deployment summary and in the `github-pages` environment.

## Repository structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml   # the only relevant content in this repo
└── README.md
```

There is intentionally nothing else here: no `docs/`, no `properdocs.yml`,
no source content. Everything needed to build the site is fetched fresh
from Codeberg on every manual run.

## Related

- **Source code (Codeberg):** https://codeberg.org/inigochoa/server-inigochoa-me
- **Live site:** https://server.inigochoa.me/
