# Releasing Patch Shorebird

These are the steps needed to create a new release:

1. Create a new release in GitHub
   - Click on `Releases -> Draft a new release`
1. Make sure to name the release `v<VERSION>` (e.g. v1.2.3)
1. Click on `Generate release notes`
1. Click on `Publish release`
1. Update the current major version tag to point to the latest release. For example, if we just released v0.1.2, we'll need to make sure the `v0` tag points to the same commit as `v0.1.2`
   ```sh
   # force update the latest release tag
   git tag -f v0 v0.1.2
   # push the tag
   git push origin :refs/tags/v0
   ```
