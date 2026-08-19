# isaiahngou.com

Personal site. [Hugo](https://gohugo.io) + the [terminal](https://github.com/panr/hugo-theme-terminal)
theme, built by GitHub Actions and served from GitHub Pages behind Cloudflare.

**The repo is the CMS.** There is no admin UI, no database, and no service to keep running.
Posts are markdown files under version control, and a commit is the publish action.

## Publishing a post

```sh
hugo new blog/my-post.md     # creates content/blog/my-post.md from archetypes/default.md
hugo server -D               # preview at localhost:1313, -D includes drafts
```

Write the post, set `draft = false` in the front matter, then commit and push to `main`.
`.github/workflows/hugo.yaml` rebuilds and deploys automatically — live in about a minute.

Front matter the theme reads:

| Key | Purpose |
|---|---|
| `title`, `date`, `author` | Post header and list metadata |
| `draft` | `true` hides it from the build; flip to `false` to publish |
| `tags` | Also populates `/tags/` |
| `description` | Blurb on list pages; without it Hugo auto-summarizes |
| `showFullContent` | Render the whole post inline on list pages |

Posts live in `content/blog/` because `contentTypeName = "blog"` in `hugo.toml`. They appear
both at `/blog/` and on the homepage, below the intro from `content/_index.md`.

### Writing without a clone

Create or edit the file directly on github.com, or press `.` in the repo to open github.dev.
Committing from the browser triggers the same deploy — useful from a phone.

## Known console noise

These three browser-console errors are **not** site bugs. Verified; don't go chasing them again.

| Message | Source |
|---|---|
| `beacon.min.js` CORS failure + `integrity` mismatch | Cloudflare Web Analytics, injected at the edge. Ad blockers and Firefox's Enhanced Tracking Protection block the request, leaving an empty body — the reported hash `z4PhNX7v…SfaPg==` is the SHA-512 of the empty string. Harmless; visitors without a blocker load it fine. |
| `Failed to get subsystem status` / `UNSUPPORTED_OS` at `content-script.js` | A browser extension, not a file this site serves. |
| `Content at https://isaiahngou.com/ may not load or link to file:///` | Same extension. No `file:` URL exists anywhere in the built site. Firefox attributes extension-injected content to the page's origin. |

To confirm: load the site in a private window with extensions disabled — all three disappear.

## Local development

```sh
git clone --recurse-submodules git@github.com:isaiahoh/isaiahoh.github.io.git
hugo server -D
```

The theme is a git submodule pinned to a commit; `--recurse-submodules` (or
`git submodule update --init`) is required or the build has no layouts.
