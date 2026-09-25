---
name: check-dependency-hallucination
description: Checks whether a Python (pip/PyPI), npm/npx (Node), or Go (go.mod) package name is AI-hallucinated or slopsquatted (a plausible-looking name that doesn't exist on the real registry, which an attacker could register to serve malware to anyone who installs it). Use before adding a new dependency to requirements.txt, pyproject.toml, package.json, or go.mod, or whenever a package name you're about to install looks unfamiliar, was suggested by a model, or you're not 100% sure it's real. Runs the open-source slopcheck (Python/npm) or modslop (Go) CLI.
---

Before installing or committing a new Python, npm, or Go dependency, verify the package name actually exists and isn't a plausible-but-fake (slopsquatted) name.

1. Detect the ecosystem from the file that changed or the install command about to run:
   - Python: `requirements.txt`, `pyproject.toml`, `Pipfile`, `pip install <pkg>`
   - npm/Node: `package.json`, `npm install <pkg>`, `npx <pkg>`
   - Go: `go.mod`, `go get <module>`

2. Make sure the right tool is available, installing it if not (both are no-signup, single-command installs):
   - Python/npm: `pip install --quiet slopcheck` (or `pipx install slopcheck`), then `slopcheck <path-to-requirements.txt-or-package.json>`
   - Go: `go install github.com/experimental-gains/modslop@latest`, then `modslop <path-to-go.mod>` (or with no argument from the module root)

3. Run the tool against the actual changed file (not a guess at the filename) and read its exit code and report, don't just skim stdout. A nonzero exit or a flagged package name means: stop, do not install it yet, and tell the user which name was flagged and why (not found on the registry / recently registered / name-collision risk) before proceeding.

4. If the tool itself isn't installable in this environment (no network, no pip/go), say so explicitly rather than skipping the check silently, and fall back to manually checking the package exists at https://pypi.org/project/<name>/, https://www.npmjs.com/package/<name>, or https://pkg.go.dev/<module> before installing.

Source: https://github.com/experimental-gains/slopcheck and https://github.com/experimental-gains/modslop (MIT).
