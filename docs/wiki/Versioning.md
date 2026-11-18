Written against v1.1.0.

How a change to this package is numbered. Consumers require it with a caret constraint (`^1.0`), so a patch or minor release reaches them on their next `composer update` and a major release does not.

| Change | Release |
| --- | --- |
| A patch's content changes, same upstream pin | patch |
| A patch is added or retired, or the pin moves within the upstream major | minor |
| The pin moves to a new upstream major | major |

For example, moving the `orchestra/testbench-core` pin from `11.6.0` to a later `11.x` release is a minor release, and moving it to `12.x` is a major one.

Published releases are listed on the [Releases](https://github.com/UAMS-Web/patches-orchestra-testbench-core/releases) page.

## Reference

- [Patches](Patches.md): the current pins and the patch list.
- [Project](Project.md): how a patch is retired.
