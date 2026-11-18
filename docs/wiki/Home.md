`uams-web/patches-orchestra-testbench-core` is a Composer package of patches that UAMS-Web repositories apply to [`orchestra/testbench-core`](https://github.com/orchestral/testbench-core) and `laravel/framework` with [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches) 2.x. Each patch stops parallel test workers colliding on a shared file, which fails tests on Windows.

This wiki holds the reference for the patch set, how to check it against its upstream releases, how the package is versioned, and troubleshooting. The [README](https://github.com/UAMS-Web/patches-orchestra-testbench-core#readme) keeps the overview, installation, and what each patch fixes and why.

## Getting started

- [Installation (README)](https://github.com/UAMS-Web/patches-orchestra-testbench-core#install): add the package to an application or an add-on.

## Guides

- [Verifying the patch set](Verifying-the-patch-set.md): apply each patch to the upstream release it is written against.

## Troubleshooting

- [Troubleshooting](Troubleshooting.md): one entry per symptom, with its cause and fix.

## Reference

- [Patches](Patches.md): the release each patch targets, the order they apply in, and the changes and tickets behind them.
- [Versioning](Versioning.md): which change makes a patch, minor or major release.

## Project

- [Project](Project.md): where the package is developed, how a patch is retired, and how to report a problem.
