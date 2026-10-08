# `wordfence-feed`

This repository publishes the shared Wordfence Intelligence vulnerability feed
used by the FirstTracks site fleet. It is an artifact repository: the source tree is
intentionally just documentation and the rolling release asset.

## What changes here affect

- The release named `wordfence-feed` must publish
  `vulnerabilities_production.json.gz`: the raw Wordfence Intelligence v3
  production feed, gzipped (github-actions#1672).
- Fleet maintenance downloads that asset anonymously and scans against it with
  `_shared/wordfence-vuln-scan.py`. No Wordfence CLI and no license are involved.
- A missing or unusable asset is not harmless. There is no fallback: every
  site's vulnerability scan reports FAILED until the asset is back.
- Keep the asset unmodified. Redistribution under the Wordfence Intelligence
  Terms depends on each record's copyright notice and license staying with it.
- The feed is public vulnerability data. Do not add credentials, customer data,
  site exports, or private operational details here.

## Safe workflow

- Read `README.md` before changing the artifact name, release tag, or consumer
  URL. Those are cross-repository contracts with `github-actions` and the site
  maintenance action.
- Do not add a checked-in copy of the large feed. Its producer in
  `github-actions` fetches and publishes it on the daily schedule.
- Treat release and workflow changes as fleet-facing changes. A green diff is
  not proof that a consuming site can download or use the asset.
- Do not test by forcing a fleet scan or by making direct Wordfence API calls.
  Those operations belong to the producer and consumer workflows.

## Validation

- Run `git diff --check` for documentation or metadata changes.
- Confirm the release asset name and download URL still match `README.md` and
  the consumer workflow before merging.
- For changes to the publishing workflow or asset contract, validate the
  corresponding `github-actions` workflow and a single consumer path through
  its normal CI; report that integration as unverified if it was not run.

There is no local application test suite in this repository. Do not invent one
that pretends to validate the remote release or the Wordfence service.
