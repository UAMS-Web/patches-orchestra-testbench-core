Written against v1.1.0.

- [A parallel test run on Windows fails with "Access is denied"](#a-parallel-test-run-on-windows-fails-with-access-is-denied)
- [A patch fails to apply](#a-patch-fails-to-apply)
- [The patches are not applied in an application](#the-patches-are-not-applied-in-an-application)
- [The patches are not found in a consumer with its own vendor directory](#the-patches-are-not-found-in-a-consumer-with-its-own-vendor-directory)

## A parallel test run on Windows fails with "Access is denied"

A test fails while booting, or while resolving a real-time facade, with an error naming `bootstrap/cache/services.php`, `bootstrap/cache/packages.php` or `storage/framework/cache/facade-{hash}.php`, and "Access is denied".

**Cause:** parallel workers are writing the same file, and Windows refuses to replace a file another process has open. These are the collisions the package's three patches remove; see [Patches](Patches.md).

**Fix:** confirm the patches are applied in the consumer. `composer install` lists each patch as it applies it. If they are missing, see [The patches are not applied in an application](#the-patches-are-not-applied-in-an-application). If they are applied and the error names another file, the collision is one the package does not cover yet: [report it](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues).

## A patch fails to apply

`composer install` or `composer update` stops with an error that a patch could not be applied to `orchestra/testbench-core` or `laravel/framework`.

**Cause:** the installed release is not the one the patch is written against. The `orchestra/testbench-core` patches are written against `v11.6.0`, which the package pins exactly. The `laravel/framework` patch is written against `v13.42.0`, but the package allows `^13.42.0`, so a later release that changes the code the patch touches makes it fail.

**Fix:** check which release is installed with `composer show laravel/framework` (or `orchestra/testbench-core`). Until the package is updated for that release, keep the consumer on a release the patch applies to, and [report it](https://github.com/UAMS-Web/patches-orchestra-testbench-core/issues) with the release named. [Verifying the patch set](Verifying-the-patch-set.md) shows how to apply a patch to a given tag by hand.

## The patches are not applied in an application

The package is installed, but `composer install` applies none of its patches.

**Cause:** `cweagans/composer-patches` applies a dependency's patches only from `packages` in `composer.lock`. An application that requires the package with `--dev` keeps it under `packages-dev`, where its patches are not read.

**Fix:** an application requires the package in `require`, as the [README](https://github.com/UAMS-Web/patches-orchestra-testbench-core#install) shows. An add-on is the exception: it requires the package with `--dev` and lists the patch files in its own root `patches.json`.

## The patches are not found in a consumer with its own vendor directory

`composer install` applies none of the package's patches, or stops with an error naming a patch path under `vendor/uams-web/patches-orchestra-testbench-core/patches/`.

**Cause:** the patch paths in the package are relative to the consumer's root and start `vendor/uams-web/patches-orchestra-testbench-core/patches/`. A consumer whose `vendor-dir` is not `vendor` has the files somewhere else.

**Fix:** none in the package today; a non-default `vendor-dir` is not covered.
