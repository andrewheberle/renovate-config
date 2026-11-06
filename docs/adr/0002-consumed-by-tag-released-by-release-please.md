# Consumed by tag, released by release-please

A change to a Shared preset changes Renovate's behaviour in every Consumer, with no commit in the Consumer to show for it. So Consumers extend a Shared preset pinned to a release tag (`github>andrewheberle/renovate-config#v1.2.3`) rather than the default branch, and this repository is released with release-please the same way onms-grpc-receiver is: conventional commit PR titles checked by `pr-title`, squash merges, and a release PR maintained with a GitHub App token so it runs the required checks. Tags are `vX.Y.Z` (release type `simple`) and releases are published rather than drafted, as no build attaches assets to them. Releases started at v1.0.0, because a Consumer depended on the first one.

A Shared preset has no API, so a version bump means:

- **Major (`feat!`)**: removes a rule, or changes behaviour a Consumer relies on, such as dropping a `fix` override or automerging something a person used to merge.
- **Minor (`feat`)**: adds a rule, or extends a rule to more updates.
- **Patch (`fix`)**: corrects a rule to do what it already meant to do, or changes only descriptions or documentation.

Renovate's `renovate-config` manager, enabled by `config:recommended`, opens PRs in Consumers to bump the pinned tag, looking up tags rather than GitHub releases. It only updates presets that are already pinned: `github>andrewheberle/renovate-config` with no tag is skipped, so a new Consumer pins the current tag by hand once. Because of the bump rules above, the Shared preset itself automerges minor and patch bumps of its own tag in Consumers, while major bumps wait for a person. Those bumps are not Shipping updates (ADR 0001), so they stay `chore(deps)` and do not release the Consumer.

`validate` runs `renovate-config-validator --strict --no-global` over every Shared preset and this repository's own `renovate.json`, and branch protection requires it along with `pr-title`. `--no-global` validates the files as repository config rather than self-hosted global config, and `--strict` also fails on config that needs migrating. It catches unknown options, wrong types and broken JSON, but not unknown values: a misspelt manager name in `matchManagers` passes, and the rule silently matches nothing. The validator's version is pinned in the workflow and updated by Renovate, which runs here with its own `renovate.json` rather than extending this repository's presets: those rules are for Consumers, and a preset consuming itself hides where behaviour comes from. Validator and workflow updates change nothing a Consumer receives, so they are `chore(deps)` and never release this repository.

## Considered Options

- **Extending the default branch**: no tags to bump, but every merge here changes every Consumer at once, and nothing in the Consumer records when its behaviour changed.
- **Pinning by commit SHA**: just as reproducible, but the `renovate-config` manager only follows tags, and a SHA says nothing about whether a change is breaking.
