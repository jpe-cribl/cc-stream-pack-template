# `.github/` — workflow callers for Cribl packs

The workflows in this directory are thin callers. Each one runs a job by calling
a reusable workflow from a shared org repository at a pinned ref. The shared
workflows provide the CI behavior and shared scripts.

## Installation

These caller workflows are copied when a pack is scaffolded from the template.
Shared CI scripts are checked out under `.gh-shared/` in CI.

Set the pack type in `test.yml` when scaffolding a pack:

```yaml
with:
  pack_type: stream   # use edge for Cribl Edge packs
```

## Usage

| File | Runs on | Calls |
| --- | --- | --- |
| `ci-gate.yml` | PR opened, synchronized, or reopened | `_ci-gate.yml` in `JacobPEvans/.github`; reports `gate / Merge Gate` |
| `test.yml` | PR touching `default/`, `data/samples/`, `tests/`, `scripts/`, `package.json`; push to `main` | `cribl-pack-test.yml` in `jpe-cribl/.github` |
| `e2e.yml` | Manual dispatch; PR touching `e2e/**` | `cribl-pack-e2e.yml` in `jpe-cribl/.github` |
| `pack-release.yml` | Tag `vX.Y.Z`, or `vX.Y.Z-RC<n>` / `vX.Y.Z-TC<n>` | `cribl-pack-release.yml` in `jpe-cribl/.github`; not run on the template |
| `release.yml` | Push to `main`; PR lifecycle events | `_release.yml` in `JacobPEvans/.github`; not run on the template |
| `review-thread-resolver.yml` | PR synchronized; PR review submitted | `review-thread-resolver.yml` in `JacobPEvans/.github-ai` |

Packs keep only the callers they use.

## Updating shared workflow inputs

Adding or renaming an input can affect every consumer pack. Keep input defaults
backward compatible whenever possible.

## Pinning external actions

Per [`SECURITY.md`](https://github.com/JacobPEvans/.github/blob/main/SECURITY.md):

- Trusted (`actions/*`, `pnpm/action-setup`, `softprops/action-gh-release`):
  semver tag pins (`@v4`, etc.)
- Untrusted (everything else): SHA pins. Renovate (per the org's
  `renovate.json`) auto-converts version tags to SHA on first run.
