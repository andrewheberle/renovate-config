# renovate-config

This repository contains shared Renovate configuration used in my own projects.

## Usage

To use the configuration in another repository add the following in the Renovate configuration:

<!-- x-release-please-start-version -->
```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>andrewheberle/renovate-config#v1.1.0"]
}
```
<!-- x-release-please-end -->

An EmDash site also extends the `emdash` preset, which turns off Renovate updates for `emdash` and the `@emdash-cms/*` packages:

<!-- x-release-please-start-version -->
```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>andrewheberle/renovate-config#v1.1.0",
    "github>andrewheberle/renovate-config:emdash#v1.1.0"
  ]
}
```
<!-- x-release-please-end -->

The site must then run `upgrade-emdash` itself, usually from a scheduled workflow that opens a PR. Without that workflow, EmDash never updates. See `docs/adr/0003` for why.

Pin a release tag rather than the default branch. Renovate's `renovate-config` manager only bumps presets that are already pinned to a tag, and minor and patch bumps automerge. See `docs/adr/` for why the rules are what they are, and `GLOSSARY.md` for the terms used.
