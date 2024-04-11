# Shorebird Patch

[![ci](https://github.com/shorebirdtech/shorebird-patch/actions/workflows/main.yaml/badge.svg)](https://github.com/shorebirdtech/shorebird-patch/actions/workflows/main.yaml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

Create a new patch using the [Shorebird CLI](https://github.com/shorebirdtech/shorebird) for use in GitHub Actions.

## Features

✅ Create new Android patches

✅ Create new iOS patches

✅ Outputs the patch number

## Usage

```yaml
steps:
  - uses: shorebirdtech/setup-shorebird@v0
  - uses: shorebirdtech/shorebird-patch@v0
    id: shorebird-patch
    with:
      platform: android
      working-directory: ./path/to/app

  - run: echo patch-number ${{ steps.shorebird-patch.outputs.patch-number }}
    shell: bash
```

## Inputs

The action takes the following inputs:

- `args`: Any arguments to pass to `shorebird patch`.
  - Use an extra `--` to pass arguments to Flutter (e.g. `-- --dart-define=KEY=VALUE`)
- `platform`: Which platform to create a patch for (e.g. `android` or `ios`)
- `working-directory`: Which directory to run `shorebird patch` in.

## Outputs

The actions outputs the following:

- `patch-number`: The number of the patch that was successfully created.
