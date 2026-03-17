# Local EchoVault Fork Notes

This local fork exists to preserve semantic-search tuning for the `memory` CLI used by Codex.

## Why this fork exists

The stock `echovault` package was patched locally to improve project-scoped semantic retrieval:

- filter noisy stopwords from FTS queries
- convert vector distance into a bounded positive similarity score
- over-fetch vector candidates when project/source filters are active
- reduce the ranking impact of low-signal diagnostic and temporary probe memories
- let the semantic top result outrank a single conflicting lexical hit in sparse-search cases

These changes are implemented in:

- `src/memory/db.py`
- `src/memory/search.py`

## Expected install mode

Install from this repo in editable mode so future `pipx` upgrades from PyPI or GitHub do not overwrite the tuned search behavior:

```bash
pipx install --force --editable /Volumes/Flash500Gb/MAC/.codex-local/echovault-fork
```

## Maintenance

- This fork is published at `git@github.com:Den4ikLucky/echovault.git`.
- `origin` should point to your fork and `upstream` should point to `https://github.com/mraza007/echovault`.
- If you pull upstream changes later, re-run semantic retrieval tests before trusting the merge.
- The active memory config still lives outside the repo at `~/.memory/config.yaml`.

## Safe sync workflow

Refresh local references:

```bash
git fetch upstream
git fetch origin
```

Rebase the tuning branch onto the latest upstream main:

```bash
git checkout codex/semantic-search-tuning
git rebase upstream/main
```

If the rebase succeeds, push the updated branch back to your fork:

```bash
git push --force-with-lease origin codex/semantic-search-tuning
```

Reinstall the editable CLI only if needed:

```bash
pipx install --force --editable /Volumes/Flash500Gb/MAC/.codex-local/echovault-fork
```

## Secret hygiene

- Do not commit `~/.memory/config.yaml` or anything under `~/.memory/`.
- Do not commit `.env` files, private keys, or local SQLite/DB artifacts.
- The repo `.gitignore` is hardened to block common local secret and state files.
