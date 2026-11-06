# renovate-config

Shared Renovate configuration that other repositories extend, so update behaviour and its rationale live in one place.

## Language

**Shared preset**:
A Renovate configuration file in this repository that other repositories extend, such as `default.json`.
_Avoid_: Shared config, base config, template

**Consumer**:
A repository that extends a Shared preset, pinned to a release tag of this repository.
_Avoid_: Downstream repo, client, user

**Shipping update**:
A dependency update that changes the artifact a Consumer releases, such as its container base image or the Go toolchain that compiles its binary.
_Avoid_: Release-triggering update, runtime update
