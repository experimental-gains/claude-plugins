# supplychain-guard

A Claude Code plugin that catches AI-hallucinated/slopsquatted package names
and Go module supply-chain leaks before they ship.

## Skills

- `check-dependency-hallucination` — verifies a new Python, npm, or Go
  dependency name is real before it's installed or committed. Runs
  [slopcheck](https://github.com/experimental-gains/slopcheck) (Python/npm)
  or [modslop](https://github.com/experimental-gains/modslop) (Go).
- `diagnose-go-fetch-failure` — distinguishes a temporary
  `proxy.golang.org` negative-cache/indexing-lag failure from a genuinely
  nonexistent module or version. Runs
  [goproxycheck](https://github.com/experimental-gains/goproxycheck).
- `audit-private-go-leak` — checks a Go module or workspace for private
  module paths that could leak to the public proxy/checksum database. Runs
  [goprivaudit](https://github.com/experimental-gains/goprivaudit).

All three trigger automatically when the situation in their description
applies (new dependency, ambiguous `go get` failure, or a `go.mod`/`go.work`
change in a repo with private modules) — no manual invocation needed, though
you can also run them explicitly.

## Install

```
claude plugin marketplace add experimental-gains/claude-plugins
claude plugin install supplychain-guard@experimental-gains-plugins
```

## License

[MIT](../../LICENSE)
