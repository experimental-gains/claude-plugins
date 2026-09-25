# claude-plugins

Claude Code plugin marketplace from [experimental-gains](https://github.com/experimental-gains).

## Add this marketplace

```
claude plugin marketplace add experimental-gains/claude-plugins
```

## Plugins

- **[supplychain-guard](plugins/supplychain-guard)** — catches AI-hallucinated/slopsquatted
  package names and Go module supply-chain leaks before they ship. Wraps
  [slopcheck](https://github.com/experimental-gains/slopcheck),
  [modslop](https://github.com/experimental-gains/modslop),
  [goprivaudit](https://github.com/experimental-gains/goprivaudit), and
  [goproxycheck](https://github.com/experimental-gains/goproxycheck).

  ```
  claude plugin install supplychain-guard@experimental-gains-plugins
  ```

## Also works as a GitHub Copilot CLI marketplace

Copilot CLI reads the same `.claude-plugin/marketplace.json` file above —
no changes needed here. Verified end-to-end with the real published repo:

```
copilot plugin marketplace add experimental-gains/claude-plugins
copilot plugin install supplychain-guard@experimental-gains-plugins
```

## Why

Coding agents occasionally invent package names that sound plausible but
don't exist. For most registries that's just an install failure; for Go
modules specifically, anyone can stand up a real repo at the exact path an
LLM hallucinated and it will resolve and build — a live "slopsquatting" risk.
`supplychain-guard`'s skills run before a suspicious dependency gets
installed or committed, using the CLIs above instead of trusting the name on
sight.

## License

[MIT](LICENSE)
