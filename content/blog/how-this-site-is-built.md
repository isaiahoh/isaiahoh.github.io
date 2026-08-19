+++
title = "How this site is built"
date = "2026-08-19"
author = "Isaiah Ngou"
draft = false
tags = ["meta"]
description = "Markdown in a git repo, a static binary, and no database in sight."
+++

There's no CMS behind this site, no database, and nothing running on a server. The repo *is* the CMS.

A post is a markdown file in `content/blog/`. I write it, commit it, and push. A GitHub Actions workflow runs Hugo, which turns the markdown into plain HTML, and publishes the result to GitHub Pages. Start to finish it takes about a minute, and there's nothing to keep patched between posts.

The appeal isn't minimalism for its own sake — it's that the failure modes are boring. Static files can't have a database outage or an expired API token. Posts are version-controlled alongside everything else, so the history of what I wrote and when is just the git log. And publishing needs no more infrastructure than pushing a commit.

The whole stack:

- **[Hugo](https://gohugo.io)** — a single static binary that renders markdown into HTML at build time
- **[terminal](https://github.com/panr/hugo-theme-terminal)** by panr — the theme
- **GitHub Actions → GitHub Pages** — builds on push, serves the output
- **Cloudflare** — DNS and TLS in front of it

That's it. No runtime, no dependencies to audit, nothing to restart.
