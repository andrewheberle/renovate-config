# renovate-config

This repository contains shared Renovate configuration used in my own projects.

## Usage

To use the configuration in another repository add the following in the Renovate configuration:

<!-- x-release-please-start-version -->
```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>andrewheberle/renovate-config#v1.0.0"]
}
```
<!-- x-release-please-end -->

Pin a release tag rather than the default branch. Renovate's `renovate-config` manager only bumps presets that are already pinned to a tag, and minor and patch bumps automerge. See `docs/adr/` for why the rules are what they are, and `GLOSSARY.md` for the terms used.
