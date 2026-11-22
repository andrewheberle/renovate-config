# EmDash updates bypass Renovate

An EmDash upgrade is more than a version bump. `upgrade-emdash` moves `emdash` and the `@emdash-cms/*` packages together (`@emdash-cms/cloudflare` requires the exact matching `emdash`), refreshes the project's EmDash agent skills, and writes `.emdash/UPGRADE.md`: the changelog entries crossed by the upgrade, the core database migrations it adds, and the project's build and migrate commands. It works out the changelog range and the added migrations by comparing the installed packages before and after it installs the new ones, so it has to do the bump itself. Run on a branch Renovate has already bumped, it finds nothing to do and writes no work order. The `emdash` Shared preset therefore turns these packages off in Renovate, and each Consumer that extends it runs `upgrade-emdash` on a schedule instead, opening its own PR with the work order in the description.

## Consequences

The preset only removes updates; it does not add the workflow that replaces them. A Consumer that extends `emdash` without an EmDash upgrade workflow never updates EmDash, and nothing tells it so. The rule's `description` and the README say so, as the only guard.

The rule keeps `groupName: "EmDash"` although it is disabled, so a Consumer that turns it back on with `"enabled": true` gets the packages bumped together rather than an install that fails on the exact peer dependency.

The packages are matched by name, so a third-party package outside the `@emdash-cms` scope (such as a community plugin) is still updated by Renovate.

## Considered Options

- **Renovate's PR triggers the updater**: the workflow resets the package files on Renovate's branch to the default branch and runs `upgrade-emdash` there. It works, but Renovate's bump is always thrown away, and the updater can then only run where the PR's checks run.
- **Renovate `postUpgradeTasks`**: running `upgrade-emdash` after Renovate's own bump has the same before-and-after problem, and the hosted Renovate app does not run arbitrary commands.
- **`dependencyDashboardApproval` instead of `enabled: false`**: new versions would stay visible on the Dependency Dashboard even in a Consumer without the workflow, but approving one opens a plain bump with no work order, which is the upgrade this preset exists to avoid.
