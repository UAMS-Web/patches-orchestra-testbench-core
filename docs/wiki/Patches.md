Written against v1.1.0.

The patch set at a glance: the upstream release each patch is written against, the order they apply in, the file each one changes, and the changes and tickets behind them. What each patch fixes and how is in the [README's Patches section](https://github.com/UAMS-Web/patches-orchestra-testbench-core#patches), which is kept beside the patch files so the two change together.

## Upstream pins

| Upstream package | Constraint in `composer.json` | Patches written against |
| --- | --- | --- |
| `orchestra/testbench-core` | exactly `11.6.0` | `v11.6.0` |
| `laravel/framework` | `^13.42.0` | `v13.42.0` |

The `laravel/framework` constraint allows later releases, so its patch fails to apply if a later release changes the code it touches. See [A patch fails to apply](Troubleshooting.md#a-patch-fails-to-apply).

## The patches, in order

The patches apply in the order `composer.json` lists them, which is the order below. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it.

| Order | File | Upstream package | File it changes | Added by | Retirement ticket |
| --- | --- | --- | --- | --- | --- |
| 1 | `testbench-parallel-services-manifest.patch` | `orchestra/testbench-core` | `src/Concerns/CreatesApplication.php` | [`a2e4cbf`](https://github.com/UAMS-Web/patches-orchestra-testbench-core/commit/a2e4cbf), reworked in [#4](https://github.com/UAMS-Web/patches-orchestra-testbench-core/pull/4) | [#1](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues/1) |
| 2 | `testbench-parallel-packages-manifest.patch` | `orchestra/testbench-core` | `src/Foundation/PackageManifest.php` | [#7](https://github.com/UAMS-Web/patches-orchestra-testbench-core/pull/7), for [#5](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues/5) | none open |
| 3 | `laravel-facade-stub-rename.patch` | `laravel/framework` | `src/Illuminate/Foundation/AliasLoader.php` | [#8](https://github.com/UAMS-Web/patches-orchestra-testbench-core/pull/8), for [#6](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues/6) | none open |

[#1](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues/1) retires the first patch once an `orchestra/testbench-core` release carries the fix. The offer to upstream ([#9](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues/9)) is closed with no upstream pull request posted.

## Reference

- [Verifying the patch set](Verifying-the-patch-set.md): apply each patch to its upstream release.
- [Versioning](Versioning.md): which release a change to this list makes.
