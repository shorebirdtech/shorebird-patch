# Releasing Shorebird Patch

Users reference this action as `@v1`, a tag we move to each new v1.x.y. Moving it ships the release to everyone, so tag the exact version first, verify it, and move `v1` last.

## Maintenance

This action is in maintenance mode. It's a thin wrapper around the [Shorebird CLI](https://github.com/shorebirdtech/shorebird), which any workflow can also run directly, so it isn't getting new features or a v2. It is kept working: CI stays green and [zizmor](https://github.com/zizmorcore/zizmor)-clean, real bugs get fixed, and fixes get released rather than left on `main`.

Prefer fixes that keep v1's behavior over behavior changes: existing workflows depend on it as it is. That includes how `args` is split and expanded: workflows pass things like `$HOME/...` in `args`, so shell expansion there has to keep working.

## Steps

1. Confirm the `ci` workflow is green on the `main` commit you are releasing. Nothing else depends on this action, so its own e2e there is the downstream check.
1. Create the release (patch bump for fixes, minor for new inputs; a breaking change needs a new major):
   ```sh
   gh release create v1.2.3 --target <main sha> --generate-notes
   ```
1. Move the major tag to it. Force-move `v1`; never delete it, since every workflow using `@v1` fails while it's missing:
   ```sh
   git fetch --tags --force
   git tag -f v1 v1.2.3
   git push -f origin refs/tags/v1
   ```

To roll back, point `v1` at the previous release (`git tag -f v1 v1.2.2 && git push -f origin refs/tags/v1`).
