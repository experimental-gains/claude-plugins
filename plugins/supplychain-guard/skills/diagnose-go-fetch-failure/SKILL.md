---
name: diagnose-go-fetch-failure
description: Diagnoses a Go module fetch failure — go get/go build/go mod tidy erroring with "unknown revision", "not found", a checksum mismatch, or a proxy timeout — by distinguishing a temporary negative-cache entry, proxy indexing lag, a genuinely nonexistent module/version, and sumdb lag. Use when a go.mod change fails to build with an ambiguous error from proxy.golang.org or sum.golang.org, especially right after a new module or tag was published. Runs the open-source goproxycheck CLI.
---

When `go get`, `go build`, or `go mod tidy` fails with an ambiguous module-fetch error (not a clear compile error), don't assume the module/version doesn't exist — proxy.golang.org's negative cache and indexing lag produce the same symptom as a real typo or an unpublished version.

1. Install if missing: `go install github.com/experimental-gains/goproxycheck@latest`
2. Run it against the exact failing `module@version` (or with no arguments from inside the module to read it from go.mod): `goproxycheck <module>@<version>`
3. Read its diagnosis, not just its exit code — it distinguishes negative-cache (retry after a wait), indexing lag (retry after a longer wait), module-unknown (genuinely doesn't exist, check the name/version for a typo), and sumdb lag (checksum database hasn't caught up yet).
4. Report the specific diagnosis to the user before suggesting a fix — "wait and retry" and "this module/version doesn't exist, check the name" call for different next steps.

Source: https://github.com/experimental-gains/goproxycheck (MIT).
