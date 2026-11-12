# uams-web/patches-orchestra-testbench-core

Patches that UAMS-Web repositories apply to [`orchestra/testbench-core`](https://github.com/orchestral/testbench-core) with [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches) 2.x. One package replaces the copies each repository carried, so a patch is changed once and retires everywhere in one release.

This package pins `orchestra/testbench-core` to exactly `11.6.0`. Every patch is written against that release.

## Install

This package is read from GitHub with a `vcs` repository, not from Packagist. The consumer's root `composer.json` lists it (Composer reads `repositories` only from the root project):

```json
"repositories": [
  { "type": "vcs", "url": "https://github.com/UAMS-Web/patches-orchestra-testbench-core.git" }
]
```

An application requires it in `require` (the plugin applies a dependency's patches only from `packages` in `composer.lock`):

```bash
composer require --no-plugins uams-web/patches-orchestra-testbench-core:^1.0
```

A package that another repository installs (an add-on) requires it with `--dev` and lists the patch files in its own root `patches.json`. The full procedure, including `composer patches-relock` and `composer patches-repatch`, is the `upstream-patch-lifecycle` skill's *Moving a consumer to the package*.

Patch paths are relative to the consumer's root and start `vendor/uams-web/patches-orchestra-testbench-core/patches/`, so a consumer with a non-default `vendor-dir` is not covered.

## Patches

Applied in this order, which is the order in `composer.json`. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it.

- **`testbench-parallel-services-manifest.patch`**: Give each parallel test worker its own service manifest. Every worker boots the same Testbench skeleton and rewrites its `bootstrap/cache/services.php`; on Windows, replacing a file another process has open fails with "Access is denied", failing whichever test was booting. Testbench names the package manifest per worker (`test_{token}_packages.php`), and the second patch below makes that apply under Pest; this names the service manifest the same way (`test_{token}_services.php`) when a parallel run has no `APP_SERVICES_CACHE` of its own. A parallel run is recognised by the worker token (`ParallelTesting::token()`), not by Testbench's `in_parallel_testing()`: Pest sets `LARAVEL_PARALLEL_TESTING` only in the parent process's `$_ENV`, so it is absent in the workers and `in_parallel_testing()` is false there. Serial runs keep the default path. A consumer that sets `APP_SERVICES_CACHE` itself keeps its value, so its own override can be removed after this package is adopted. Upstream offer: UAMS-Web/aether-multisite-forms#98.
- **`testbench-parallel-packages-manifest.patch`**: Make Testbench's own per-worker package manifest (`test_{token}_packages.php`) apply under Pest. It is chosen with `in_parallel_testing()`, which is false in Pest workers (see the first patch), so every worker shared `bootstrap/cache/packages.php` and a cold run on Windows could fail with "Access is denied". The patch chooses the name from the worker token. Serial runs keep the default path.

Each description names the ticket that tracks the upstream fix, where there is one.

## Versioning

| Change | Release |
| --- | --- |
| A patch's content changes, same upstream pin | patch |
| A patch is added or retired, or the pin moves within the upstream major | minor |
| The pin moves to a new upstream major | major |

## Verifying the set

Apply the 2 patches in order to the `v11.6.0` tag of `orchestra/testbench-core` with `git apply -p1`. Applying either patch a second time must fail.

## License

MIT, see [`LICENSE`](LICENSE). The patches modify `orchestra/testbench-core`, which is also MIT licensed.
