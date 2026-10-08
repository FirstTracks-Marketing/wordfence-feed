# wordfence-feed

Shared **Wordfence Intelligence vulnerability feed** for the site fleet.

This repository exists only to host a single, daily-refreshed artifact: the
Wordfence Intelligence v3 **production** vulnerability feed, unmodified and
gzipped, published as the `vulnerabilities_production.json.gz` asset on the
rolling **`wordfence-feed`** release.

## Why

Every site's maintenance run checks its installed plugin and theme versions
against the whole feed. The feed is **identical for every site**, and the
Intelligence API allows one feed request per key every 30 minutes, so ~100 sites
cannot each fetch it. Instead it is fetched **once per day** by
`publish-wordfence-feed` in the private `github-actions` repository, using the
org secret `WORDFENCE_INTELLIGENCE_API_KEY`, and shared here. Consumers download
it anonymously, so no site needs a Wordfence credential.

Until github-actions#1672 this was the Wordfence CLI's pickled cache file
(`vulnerability_index_production.gz`). Wordfence CLI licenses end on 2026-10-14
and are no longer issued, so the fleet stopped running the CLI. The raw JSON is
version-independent; consumers parse it with the CLI's GPLv3 package, pinned in
`github-actions/_shared/wordfence-lib/wheels.sha256`.

## Data and attribution

Vulnerability data © Defiant Inc. (Wordfence Intelligence), redistributed under
the [Wordfence Intelligence Terms and Conditions](https://www.wordfence.com/wordfence-intelligence-terms-and-conditions/).
The asset is the feed exactly as served: every record keeps its copyright notice
and license in its `copyrights` field, and its identifier resolves to a record
at <https://www.wordfence.com/threat-intel/vulnerabilities>.

## Consuming it

The `maintenance/malware-scan` action downloads:

```
https://github.com/FirstTracks-Marketing/wordfence-feed/releases/download/wordfence-feed/vulnerabilities_production.json.gz
```

and runs `_shared/wordfence-vuln-scan.py` against it. There is **no fallback**:
if the download fails, the site's vulnerability scan is reported as FAILED,
never as clean.
