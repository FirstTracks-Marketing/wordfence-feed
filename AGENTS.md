# `wordfence-feed`

This repository publishes the shared Wordfence CLI vulnerability feed used by
the FirstTracks site fleet. It is an artifact repository: the source tree is
intentionally just documentation and the rolling release asset.

## What changes here affect

- The release named `wordfence-feed` must publish
  `vulnerability_index_production.gz`.
- Fleet maintenance downloads that asset anonymously and places it in the
  Wordfence CLI cache before running `wordfence vuln-scan`.
- A missing or unusable asset is not harmless. Consumers fall back to a direct
  Wordfence Intelligence download, which restores correctness but loses the
  rate-limit protection this repository exists to provide.
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
