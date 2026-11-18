Written against v1.1.0.

Check that every patch still applies to the upstream release it is written against, in order, and that applying it a second time fails. Run this after changing a patch, and before moving an upstream pin.

1. **Clone the two upstream releases into an empty scratch directory.**

   ```bash
   git clone --depth 1 --branch v11.6.0 https://github.com/orchestral/testbench-core.git testbench-core
   git clone --depth 1 --branch v13.42.0 https://github.com/laravel/framework.git framework
   ```

   Two directories, `testbench-core` and `framework`, each at its tag.

2. **Set `PATCHES` to this repository's `patches` folder.** Replace the placeholder with the path to your checkout.

   ```bash
   PATCHES=/path/to/patches-orchestra-testbench-core/patches
   ```

3. **Apply the two `orchestra/testbench-core` patches in order.**

   ```bash
   cd testbench-core
   git apply -p1 "$PATCHES/testbench-parallel-services-manifest.patch"; echo "services: $?"
   git apply -p1 "$PATCHES/testbench-parallel-packages-manifest.patch"; echo "packages: $?"
   ```

   Each prints `0`, and `git diff --stat` reports 2 files changed.

4. **Apply them a second time.** Each must fail, which shows the first run changed the code rather than finding it already patched.

   ```bash
   git apply -p1 "$PATCHES/testbench-parallel-services-manifest.patch"; echo "services again: $?"
   git apply -p1 "$PATCHES/testbench-parallel-packages-manifest.patch"; echo "packages again: $?"
   cd ..
   ```

   Each prints `1`, after an error naming the file the patch no longer applies to.

5. **Do the same for the `laravel/framework` patch.**

   ```bash
   cd framework
   git apply -p1 "$PATCHES/laravel-facade-stub-rename.patch"; echo "facade: $?"
   git apply -p1 "$PATCHES/laravel-facade-stub-rename.patch"; echo "facade again: $?"
   cd ..
   ```

   The first prints `0` and the second `1`.

**Note:** the order in step 3 is the order in `composer.json`. A later patch is cut against the output of the earlier ones, so applying them out of order can fail even when each patch is correct.

## Reference

- [Patches](Patches.md): the upstream pins, the order, and what each patch changes.
- [A patch fails to apply](Troubleshooting.md#a-patch-fails-to-apply): what a failure in step 3 or 5 means.
