---
name: audit-private-go-leak
description: Audits a Go module or workspace for accidental leakage of private or internal module paths to the public Go module proxy and checksum database — missing GOPRIVATE/GONOSUMCHECK config, an insteadOf/replace directive that doesn't actually keep a dependency private, a go.work override that silently changes what's private, or a netrc-authenticated private module with no other private-auth signal. Use before committing go.mod or go.work changes in a repo that has any private or internal Go modules, or when wiring up a new private Go module for the first time. Runs the open-source goprivaudit CLI.
---

Before committing a go.mod or go.work change in a repo with any private/internal Go modules, check that private module paths can't leak to the public proxy or checksum database.

1. Install if missing: `go install github.com/experimental-gains/goprivaudit@latest`
2. Run it from the module or workspace root: `goprivaudit` (no arguments needed — it reads go.mod/go.work/netrc/GOPRIVATE itself)
3. Read the full report, not just the exit code — a clean exit on an unrelated check doesn't mean every private module path is covered. Pay particular attention to any module it flags as having no GOPRIVATE/insteadOf/netrc signal at all, since those are the ones that would silently hit the public proxy.
4. If it flags a leak, tell the user which module path is exposed and why (no GOPRIVATE match / go.work override / missing insteadOf) before they push — a leaked private module path can reveal internal project/company names even if the code itself never becomes fetchable.

Source: https://github.com/experimental-gains/goprivaudit (MIT).
