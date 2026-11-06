# AGENTS.md

## Project overview

This repository hosts shared Renovate configuration used by other repositories. It is intended for my own use only at this time.

## Boundaries
- Don't commit secrets, tokens, or real config values.
- Be cautions; changes to this repo affect Renovate behaviour in other repositories.
- Keep changes scoped to the task. No drive-by refactors or renames.
- Consumers pin a release tag, so a change only reaches them once released. Choose the PR title's conventional commit type by the version bump rules in `docs/adr/0002`; removing or weakening a rule is a breaking change.
- Shipping updates (see `GLOSSARY.md`) must stay `fix` commits; see `docs/adr/0001` before touching those rules.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues on andrewheberle/renovate-config, using the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Uses the default label names: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
