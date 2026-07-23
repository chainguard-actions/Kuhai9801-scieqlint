<!-- markdownlint-disable -->

# Hardening Report: Kuhai9801--scieqlint/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kuhai9801--scieqlint/v1.1.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses `actions/setup-python@v6`, which is pinned to a mutable tag (`@v6`) rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling supply-chain attacks.

Locations:

- `action.yml:14`

### script-injection (severity: high)

Sub-rule (a): The expression `${{ inputs.package-version }}` is interpolated directly into a `run:` shell command: `python -m pip install "scieqlint==${{ inputs.package-version }}"`. A caller-controlled input value is substituted into the shell command before the shell parses it, enabling command injection (e.g., a value like `1.0.0" && malicious-command #` would execute arbitrary code). The value should be passed via an `env:` variable and the shell variable should be double-quoted.

Locations:

- `action.yml:18`

### script-injection (severity: high)

Sub-rule (a): The expression `${{ inputs.args }}` is interpolated directly into a `run:` shell command: `scieqlint ${{ inputs.args }}`. A caller-controlled input value is substituted into the shell command before the shell parses it, enabling arbitrary command injection (e.g., a value like `. ; malicious-command` would execute arbitrary code). The value should be passed via an `env:` variable and the shell variable should be double-quoted.

Locations:

- `action.yml:21`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.package-version }}" appears directly in run: block of step "Install SciEqLint"; move to env: map

Locations:

- `action.yml:28`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.args }}" appears directly in run: block of step "Run SciEqLint"; move to env: map

Locations:

- `action.yml:31`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection

**Notes:**

Fixed all 5 findings in hardened/action/action.yml:
1. Pinned actions/setup-python@v6 to immutable SHA ece7cb06caefa5fff74198d8649806c4678c61a1 (# v6 comment preserved).
2. Moved ${{ inputs.package-version }} out of the 'run:' block into an 'env:' variable (PACKAGE_VERSION) and referenced it as $PACKAGE_VERSION in the pip install command.
3. Moved ${{ inputs.args }} out of the 'run:' block into an 'env:' variable (SCIEQLINT_ARGS) and used a bash array (IFS split + read -ra) to safely pass the arguments to scieqlint, preventing shell injection while preserving correct argument splitting.

