# `wordfence-feed`

A FirstTracks shared tool. It lives at `~/dev/ftm/tools/wordfence-feed` because the layout follows the
repo topics — see `~/dev/CLAUDE.md`.

> ℹ️ **This file is a measured scaffold, not authored context.** Every statement below was
> read out of this repo by `llm/bin/scaffold-context` on 2026-08-14. Nothing here was written by
> someone who knows the repo, so it says what is *true* rather than what *matters* —
> the constraint that keeps a generated file from being confidently wrong.
>
> **Replace it.** The thing worth adding is what a colleague would have to tell you and
> could not read off the filesystem: what this does, what breaks, what to be careful of.
> When you do, delete this note — and keep `CLAUDE.md` a symlink to this file rather than
> editing it, or the copies diverge (`llm/bin/emit-context`, `llm/bin/context-audit`).

## What was measured

| | |
| --- | --- |
| WPCS | **not a declared dependency** |
| PHPCS ruleset | **none in this repo** |

## Where the standards live

This repo does not carry FTM working standards; they are shared, because they were being
reinvented per repo and going stale. Install the plugin and the skills load on demand:

```bash
claude plugin marketplace add FirstTracks-Marketing/llm
claude plugin install ftm-wordpress@ftm
```

| Skill | For |
| --- | --- |
| `wp-repo-survey` | What generation this repo is at, before changing it |
| `wp-coding-standards` | Finding this repo's lint command, and wiring PHPCS where it is missing |

Estate-wide rules — worktrees, commit conventions, the fleet's hazards — are in
`~/dev/CLAUDE.md`, which loads automatically anywhere under `~/dev`.
