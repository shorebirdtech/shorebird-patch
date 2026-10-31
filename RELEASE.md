# Releasing Shorebird Patch

Users reference this action as `@v1`, a tag we move to each new v1.x.y. Moving it ships the release to everyone, so confirm CI on the exact commit, tag it, publish its release, and move `v1` last.

## Maintenance

This action is in maintenance mode. It's a thin wrapper around the [Shorebird CLI](https://github.com/shorebirdtech/shorebird), which any workflow can also run directly, so it isn't getting new features or a v2. The exception is an input that keeps `@v1` workflows working through a CLI change, such as `track` for the CLI dropping its default track. It is kept working: CI stays green and [zizmor](https://github.com/zizmorcore/zizmor)-clean, real bugs get fixed, and fixes get released rather than left on `main`.

A fix must not change behavior that existing `@v1` workflows rely on. That includes how `args` is split and expanded: workflows pass things like `$HOME/...` in `args`, so shell expansion there has to keep working. The `args` job in `.github/workflows/main.yaml` pins that behavior to v1.0.0's; a change that needs one of its expected values changed breaks `@v1` workflows.

## Steps

`vX.Y.Z` below is the new version: the last release with its patch number bumped. Releases are patch bumps for fixes, and minor bumps when they add an input.

1. Confirm the `ci` workflow is green on the `main` commit you are releasing. No other Shorebird action uses this one, so its own e2e there is the downstream check.
1. Tag that commit, and check the tag points at the commit CI passed on:
   ```sh
   git fetch origin
   git tag vX.Y.Z <main sha>
   git push origin refs/tags/vX.Y.Z
   git ls-remote origin refs/tags/vX.Y.Z   # must print <main sha>
   ```
1. Publish the release from the tag:
   ```sh
   gh release create vX.Y.Z --verify-tag --generate-notes
   ```
1. Move the major tag to it. Force-move `v1`; never delete it, since every workflow using `@v1` fails while it's missing:
   ```sh
   git fetch --tags --force
   git tag -f v1 vX.Y.Z
   git push -f origin refs/tags/v1
   ```

## A bad release

If a release turns out to be bad after `v1` moved, point `v1` back at the previous release and mark that release Latest again:

```sh
git tag -f v1 <previous vX.Y.Z>
git push -f origin refs/tags/v1
gh release edit <previous vX.Y.Z> --latest
```

Then release the fix as the next patch version. Leave the bad tag in place, since deleting it breaks any workflow pinned to it, and say in its release notes that it's superseded (`gh release edit vX.Y.Z --notes-file <file>`).
