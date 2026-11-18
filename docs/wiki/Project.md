Where the package is developed, how a patch is retired, and how to report a problem.

## Where it is developed

The package lives in [UAMS-Web/patches-orchestra-testbench-core](https://github.com/UAMS-Web/patches-orchestra-testbench-core). Changes land through a pull request to `main`, following [`CONTRIBUTING.md`](https://github.com/UAMS-Web/patches-orchestra-testbench-core/blob/main/CONTRIBUTING.md). Releases are tags on `main`, listed on the [Releases](https://github.com/UAMS-Web/patches-orchestra-testbench-core/releases) page and numbered as [Versioning](Versioning.md) describes.

## How a patch is retired

Each patch is temporary. It is removed in a minor release once an upstream release carries an equivalent fix, so every consumer drops it on its next update.

- [Patches](Patches.md) names the change that added each patch and its retirement ticket where one is open; each patch's description in `composer.json` names a ticket too.
- The `Watch upstream fixes` workflow checks the upstream issues and pull requests listed in [`.github/upstream-watches/`](https://github.com/UAMS-Web/patches-orchestra-testbench-core/tree/main/.github/upstream-watches) once a day, and comments on the tracking ticket when one moves.

## Reporting a problem

Open an issue in [the repository's tracker](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues). Name the package version, the upstream release installed (`composer show orchestra/testbench-core laravel/framework`), the platform, and the exact error.

## The wiki

These pages are kept in [`docs/wiki/`](https://github.com/UAMS-Web/patches-orchestra-testbench-core/tree/main/docs/wiki) and published by the `Publish wiki` workflow when a release is published, or by hand. Each publish replaces the whole wiki, so a page edited in the browser is overwritten.
