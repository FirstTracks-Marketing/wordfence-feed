# wordfence-feed

Shared **Wordfence CLI vulnerability feed** for the site fleet.

This repository exists only to host a single, daily-refreshed artifact: the
gzipped Wordfence CLI `vulnerability_index_production` cache entry, published as
the `vulnerability_index_production.gz` asset on the rolling **`wordfence-feed`**
release.

## Why

Every site's maintenance run does a `wordfence vuln-scan`, which downloads the
entire (~96 MiB) Wordfence Intelligence production vulnerability feed and matches
it locally. The feed is **identical for every site**, so ~100 sites fetching it
daily from fresh CI runners hammered the Intelligence API and hit rate limits
(HTTP 429). Instead, the feed is fetched **once per day** (by `publish-wordfence-feed`
in the private `github-actions` repo) and shared here. Each site downloads this
asset into its Wordfence CLI cache, turning the scan into a cache hit — **zero
Intelligence API calls per site**.

The data here is public vulnerability information (CVEs, CVSS, remediation), which
is why it can live in a public repo — consumers download it anonymously, so no
per-repo secrets are needed across the fleet.

## Consuming it

The `maintenance/malware-scan` action downloads:

```
https://github.com/FirstTracks-Marketing/wordfence-feed/releases/download/wordfence-feed/vulnerability_index_production.gz
```

decompresses it into a `--cache-directory` under the CLI's cache filename, and
runs `wordfence vuln-scan` against it. Best-effort: on any failure it falls back
to a direct Intelligence API fetch.
