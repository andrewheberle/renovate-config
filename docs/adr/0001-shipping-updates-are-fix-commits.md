# Shipping updates are fix commits

Consumers release with release-please, which cuts a release for `fix` and `feat` commits but not for `chore`. Renovate commits dependency updates as `chore(deps)` by default, so a Shipping update would change what a Consumer builds without ever producing a release that carries it. The Shared preset therefore commits Shipping updates as `fix(deps)`, so each one ships as a patch release, and automerges them only where waiting for a person adds nothing.

Two kinds of update are Shipping updates today:

- **Container base image**: the `FROM` of the final Dockerfile stage, for example `cgr.dev/chainguard/static` pinned by digest. Updates are batched to before 6am on Monday and automerge once the Consumer's required checks pass. Releases whose only changelog entry is a base image bump are expected. The rationale for releasing on every base update rather than rebuilding tags in place is in onms-grpc-receiver's ADR 0001.
- **Go toolchain**: the `toolchain` line in `go.mod`. A statically linked Go binary carries the standard library inside it, so the toolchain is as much a part of what ships as the base image. Patch updates carry Go's security fixes and automerge. Minor updates are merged by a person: they can bring new vet and lint findings, and golangci-lint rejects a toolchain newer than the one its own binary was built with until it releases a matching build. The `go` line is left alone, as Renovate treats `go 1.26.0` as `^1.26.0`, and moving it changes `GODEBUG` defaults and the minimum Go for importers. The rationale for pinning the toolchain at all is in onms-grpc-receiver's ADR 0005.

Updates that change nothing a Consumer ships, such as GitHub Actions, linters and bumps of this repository's own tag, stay `chore(deps)`.

## Consequences

The `fix` overrides are load-bearing. Removing either one is a breaking change to this repository (see ADR 0002): the Consumer's pinned base image or toolchain would keep being updated but would stop reaching releases, which is worse than not pinning at all.

Automerge relies on each Consumer requiring a build check on its default branch. A Consumer without one should not extend this preset as it stands.

`matchDepTypes: ["final"]` matches the last external image in a Dockerfile. When the final stage is built `FROM` an earlier stage (`FROM builder`), that line is not a dependency, so `final` lands on the last external image instead, which may not be the runtime base. Every current Consumer has a single external `FROM`.

The Monday schedule has no `timezone`, so Renovate reads it as UTC (about 2pm Monday in Perth). This was carried over unchanged when the rules moved here from onms-grpc-receiver; changing it would change every Consumer's timing, so it belongs in its own release.
